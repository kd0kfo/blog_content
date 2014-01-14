Listing Ports and Who is Using Them
===================================

I do this every so often, but not often enough to remember the flags. So here's the command.

.. code:: bash

   netstat -lnptu


This will list which ports are open. Also, the last column with have the PID and program name of the program using the port. Be ready for verbose output...

