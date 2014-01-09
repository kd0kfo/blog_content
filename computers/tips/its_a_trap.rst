It's a Trap!
============

I created a source-able bash script to enable error trapping and print some information about the error. Then it will exit. The code (below) may be found on github (https://github.com/kd0kfo/utils/) under bash.

.. code:: bash

   function ackbar() {
           local name="$0"
           local code="$1"
           echo ${name} errored out line with code ${code}
           echo Line Stack: ${BASH_LINENO[*]}
           exit $code
   }

   trap 'ackbar $?' ERR
