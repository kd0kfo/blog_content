Backslash in a shell
=====================

Adding a backslash before a command runs it without any existing alias.

For example, suppose the following alias was set:
     alias cp='cp -i'

Then,
     \\cp a b

will copy file a to b without asking if b should be overwritten.
