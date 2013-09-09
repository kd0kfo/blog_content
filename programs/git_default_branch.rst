Specifying a Default Branch for Git push/pull operations
========================================================

Often, it is convenient not to specify a branch when pushing or pulling from a repository. To do this, run the following commands:

.. code::

    $ git config branch.master.remote origin
    $ git config branch.master.merge refs/heads/master 


A non-default remote may be substituted in place of "origin". Same applies to the "master" branch.
