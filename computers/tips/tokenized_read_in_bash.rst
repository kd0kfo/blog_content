Tokenized Read in Bash
======================

I wanted to read a file, take each line, print the first column (tab-delimited) and then run my unixtime program[1]_ on the second column. Here's a nice way to do that.

.. code:: bash

    while read -ra line;do
        echo ${line[0]} $(unixtime ${line[1]})
        done < thefile.tsv

This is accomplished using the "-ra" flag to read. It will place each line in $line, but it will tokenize it. Just want I wanted to do.

.. [5] https://github.com/kd0kfo/utils/blob/master/unixtime.c
