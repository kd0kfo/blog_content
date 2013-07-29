Description
===========

In order to create, update and add executable code for the PIC microcontroller without the need to reprogram the chip, a machine instruction execution system was created for the PICOS, called PICLANG, which is an amalgamation of PIC LANGuage. PICLANG is Turing complete language. Using PICLANG, programs can be written and then compiled with a custom compiler, called pasm. Pasm parses the source code and generates any requested combination of PICLANG machine code (see below), Intel Hex files (http://en.wikipedia.org/wiki/Intel_HEX), PICLANG assembly files and code which can be included in a Hi-Tech C compiler code file to put the machine code directly into EEPROM.

Machine Code
============

Each machine instruction is represented by a 2-byte op code, which is enumerated in the ''piclang.h'' header file of PICOS. Currently, programs do not perform floating point arithmetic and integer arithmetic is limited to 16-bit precision. As 16-bit programs, PICLANG data are stored little endian. PICLANG machine code is divided into three sections: the Program Control Block (PCB), machine instructions and data values, and a section for static strings. PICLANG defines a unsigned integral data type, ''picos_size_t'', which is two bytes in size.

Program Control Block
---------------------

The PCB contains data needed to initialize and run the program. The parts of the PCB are:

+----------------------+-------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Name                 | Byte Size (in two-byte units) | Description.                                                                                                                                                                                                                                                                                                                                                                          |
+----------------------+-------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Magic Number         | 2                             | This is used as a naive test to see if a files does contain an executable. The hexadecimal magic numbers for PICLANG executables are 6f46 and 006f.                                                                                                                                                                                                                                   |
+----------------------+-------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Size                 | 1                             | Size, in bytes, of the executable section of the program. Max: 64KB.                                                                                                                                                                                                                                                                                                                  |
+----------------------+-------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Number of Pages      | 1                             | Number of pages in memory to be allocated to the program.                                                                                                                                                                                                                                                                                                                             |
+----------------------+-------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Program Counter (PC) | 1                             | Position, in bytes, of the running program with respect to the beginning of the executable section. The initial value of the PC is set to the beginning of the "main" function (see also Syntax).                                                                                                                                                                                     |
+----------------------+-------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Status               | 1                             | Indication of the state of the program. Possible values are Running/Successful, Suspended or various error codes.                                                                                                                                                                                                                                                                     |
+----------------------+-------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Start Address        | 1                             | Offset of executable section with respect to the beginning of the program file, excluding the magic numbers.                                                                                                                                                                                                                                                                          |
+----------------------+-------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| String Address       | 1                             | Offset of string section with respect to the beginning of the program file, excluding the magic numbers.                                                                                                                                                                                                                                                                              |
+----------------------+-------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Stack                | (Default 16)                  | This block of memory stores the stack used by functions of the PICLANG. The size is determined at the compile time of the PICOS kernel, but the default is 16 bytes. Over-writing or 'over popping' the stack will cause the program to end with the status byte being set to the error code PICLANG_SEGV.                                                                            |
+----------------------+-------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Stack Head           | 1                             | Zero-indexed count of the total number of elements on the stack. Since the stack begins at the lowest address in memory, this is also the index of the next position of a value pushed onto the stack.                                                                                                                                                                                |
+----------------------+-------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Call Stack           | (Default 8)                   | This block of memory stores the call stack used to keep track of return address when calling subroutines within PICLANG executables. The size is determined at the compile time of the PICOS kernel, but the default is 8 bytes. Over-writing or 'over popping' the call stack will cause the program to end with the status byte being set to the error code PICLANG_STACK_OVERFLOW. |
+----------------------+-------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Call Stack Head      | 1                             | Zero-indexed count of the total number of elements on the call stack. Since the call stack begins at the lowest address in memory, this is also the index of the next position of a value pushed onto the stack.                                                                                                                                                                      |
+----------------------+-------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Syntax
======

Source code grammar of PICLANG borrows from multiple languages. Functions and block structure resemble C. Subroutine usage resembles FORTRAN or assembly. C-style comments are used as well. Statements are semi-colon delimited and statement blocks are enclosed with "{" and "}" character pairs. System calls can take arguments in parenthesis following the function name, while subroutine arguments are pass along using the program stack.  

The following is a simple example program that displays the date and time in standard output:

.. code::

    /**
     * This is a program that will display the date
     */
    def <main>
    {
      putd(time('m'));
      putch('/');
      putd(time('d'));
      cr();
      putd(time('H'));
      putch(':');
      putd(time('M'));
    }


System Calls
------------

For system calls, such as putd or time, PICLANG follows the C function syntax. Primitive data types, such as characters, located in parathesis are pushed into the program stack. If a system call returns a value, like the time function does (an integer), the value is pushed into the stack. So for example,
<code>putd(time('m'));</code> would push the 'm' character onto the stack and then make the time system call. Time would pop the stack and return with the corresponding integer, in this case the current minute of the time of day, on the stack. Then putd would be called, which would pop the stack and display that integer in standard output.

Strings
-------

Strings are any number of characters contained within a pair quotation marks, such as "this is a string". When a string is used in a program, it is stored in the string section of the machine code, terminated by a null character. Within the executable section of the program, the string is referred to by the index of the first byte of the string with respect to the beginning of the string section offset by the size of the command line argument buffer. When a string is first used in source code, the compiler adds the string to a table of strings and subsequently replaces occurrences of the string in the program with the strings address. Since strings are identified by their address, the address may be stored in a variable in the program and referenced through the program. System calls that use strings will dereference the address and access the string.

Array Reference
---------------

Currently, array referencing only works with strings. Strings are null-terminated arrays of characters. Operations on strings use the index of the first character with respect to the beginning of the string section of the executable offset by the size of the command line argument  buffer. The character value referenced will be pushed onto the stack as a ''picos_size_t'' type. Syntax is exactly like the C-style array reference. The following code stores a string and then prints the character, 'W', to standard output.

.. code::

    def <main>
    {
         foo = "Hello, World!";
         bar = foo[7];
         putch(bar);
    }

The zero-index array index to be referenced is contained in square brackets. The string array starting index, which in this example is stored in ''foo'', precedes the bracket notation. Thus, in the example above, the program adds the value of foo and 7 and looks up that value in the string section of the program.

Characters
----------

Characters are indicated by being enclosed with apostrophes, such as 'm' in the code example above. It is represented in the machine code, or on the stack, by its ASCII(http://en.wikipedia.org/wiki/ASCII) value, albeit stored as a ''picos_size_t'' type.

Integral Data
-------------

Integral data may be assigned to variables. When the program is compiled each variable is assigned to a portion of page memory and the number of pages needed is set in the PCB. Assignment to the variables is done using the equal sign, '=', and using the variable as a statement, or part of a statement, will cause its value to be pushed onto the stack. So, for example, 
'''a = 3; putd(a + 2);''', will push 3 onto the stack, pop the stack into a, push a and 2 onto the stack, pop the stack twice and push the sum onto the stack, and, finally, putd will pop the stack and display the integer in standard output. Currently, there is no optimization process during program building; so the redundant process of popping 3 into a and then pushing a onto the stack will remain part of the executable.

Subroutines
-----------

Subroutines may be defined and called in PICLANG programs. The name of the subroutine is indicated by the greather than and less than signs, as shown in the code sample above. In this code sample, there is only one subroutine, called main. All programs require a main function and the program starts with this function. Definition of a subroutine is indicated by the '''def''' keyword followed by the subroutine name. After the subroutine name is a statement block. Below is an example of the usage of subroutines. There are two subroutines, test and main. The program will begin by executing code corresponding to the main function. The first line will write the string "Hello" to standard output. Then, the program will run the function test subroutine. The number 3 is passed to the subroutine, ''test'', using the stack functions push and pop. If ''test'' were to return data as well, it would use the stack functions. This example does not return data. After the data is popped into the variable, ''test_val'', it is tested against the value 4 (which will be false). If ''test_val'' had equaled 4, the subroutine would have ended at the '''return''' statement. The end of the subroutine does not require a return statement, although it can be included redundantly.

Analogous to returning from subroutines, programs' execution may be ended at any time using the exit function. If exit has an argument, that argument value is assigned to the programs status, as described in the PCB section above. Without an argument, the status is set to SUCCESS.

.. code::

    def <test>
    {
            test_val = pop();
            if(test_val == 4)
            {
              return;
            }
            sprint("in test");
    }
    
    def <main>
    {
            sprint("Hello");
            push(3);
            call <test>;
    }



Accessing Command Arguments
===========================

Command line argument access within a PICLANG program was designed to emulate C. A constant, ''argc'', will always equal the number of words, given as command line arguments, delimited by spaces. This will include the name of the program.

Like in C, PICLANG programs have an array, called ''argv'', which is an array of each word in the argument buffer. This is a zero-indexed array, with elements that are treated like strings. For example, if a program called ''echo'' was executed with the command, ''echo test this out'', then ''argv[2]'' would refer to the word ''this'' and would have the numberical value of ten. This would be used as a pointer to the specific string in the array. Therefore, ''argv[2][1]'' would equal 'h', or equivalently 0x68.

Compiler
--------
PICOS packages contain compiler tools for building PICLANG code.

picosc
------

The PICLANG code compiler is called ''picosc''. It can read files and STDIN. Compiled code may be saved in multiple formats.

Assembly: For any PICLANG source code, picosc can output the corresponding assembly code. 

Binary: This format corresponds to the machine code of PICOS. If the compiler is run in compiler mode, a library file is produced instead. This file saves all of the binary components of the source file in a format that may be linked later. This is not an executable format.

EEPROM: PICOS was built with the intention of using the Hi Tech C compilers. These compilers contain macros for loading EEPROM data. This file format saves the compiled code in a format that may be used to build this into a PIC executable using Hi Tech C compilers.

Hex: This mode saves the compiled code using the Intel Hex 32 format (http://en.wikipedia.org/wiki/Intel_HEX). This may also be useful for program microcontrolers and EEPROMs.

Linkage: This list addresses of variables and subroutines in the compiled code.

List: A list file is similar to an assembly file. However, each line contains the WORD index of each assembly command. Also, variables and subroutine labels are replaced by their addresses in memory and compiled code, respectively.

Sample Source files
===================

Both the source .tgz file and debian packages found in the download section on the PICOS page contain a collection of source files for useful programs to get started using PICOS. The files are found in the "sample_programs" subdirectory and end with the ".plc"
