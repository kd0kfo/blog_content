Readlink
========

Readlink resolves symlinks. I'm not sure how I went so long using linux without knowing about this, but it's awesome. On a system of mine, /usr/bin/java points to /etc/alternatives/java which points to /usr/lib/jvm/java-1.6.0-openjdk-1.6.0.0.x86_64/jre/bin/java. It would take two rounds of 'ls -l' to figure out where the real file is. 

Or run:

.. code:: bash

    readlink -f $(which java)


The '-f' flag follows all of the links to the real source. Without the '-f' flag, this would have returned /etc/alternatives/java.

More information at http://www.gnu.org/software/coreutils/manual/html_node/readlink-invocation.html
