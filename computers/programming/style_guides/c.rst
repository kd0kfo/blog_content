C/C++ Style Guide
=================

About
-----

This style guide applies to C and C++.

Definitions
-----------

* If a pointer is being defined as null, use NULL. If NULL is not defined, define it as (void*)0.

  * Example: double *foo = NULL;

Libraries
---------

* Use namespaces.

  * Exceptions: functions which initialize or destroy structures (no conflict if structure name is unique)

* Do not place "using" outside of a function.
* printf should be preferred over std::cout since it is much, much cleaner to read. For a good discussion, see Google's style guide (http://google-styleguide.googlecode.com/svn/trunk/cppguide.xml?showone=Streams#Streams)


Pointers
--------

* When declaring a pointer or multiple on the same line with other declared variable, put the asterisk next to the variable name without a space between it and the variable name.

  * Example: int *x,*y,z,q;

* When a pointer is declared by itself, put the asterisk next to variable name.

  * Example: int *x;
