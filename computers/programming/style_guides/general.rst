General Code Style Guide
========================

About
-----

This style guide should apply to language-agnostic programming techniques and practices. For language-specific style, see the style guide for that specific language.

Case
----

For easy of reading, use underscores between words in a variable name. There is one exception to this and that is java. See Case in the Java Code Style Guide for details.

Formatting
----------

* Code executed within if and while blocks should have a their own line.

  Good

.. code:: c
 
        while(amount_read > 0)
            amount_read = some_read_function(these_args);
..

   Bad

.. code:: c

        while(amount_read > 0) amount_read = some_read_function(these_args);


Naming
------

* Variable names with multiple words should be delimited with underscore characters, "_", rather than globing them together.

  * this_is_good  is better than thisIsBad (except for Class names)

* Variable names should start with a lower case alphabetical character.

  * this_is_good is better than This_is_bad.

* Names of boolean types should reflect the question to which their values are true, if possible.

  * Example, a boolean to indicate if a vector is empty should be called "is_empty".

* Class names with multiple words should be globed together and should start with a capital letter.

  * Example, ThisIsAClass


Version Numbers
---------------

* Format is MAJOR.MINOR.REVISION.

  * For a beta version, REVISION should always be "b". Otherwise, all three parts should be an integer.
  * REVISION should be incremented when things are fixed but a no new features are added.
  * MINOR should be incremented when at least one new feature is added.
  * MAJOR should be incremented when it is no longer compatable with past instances or if it has changed to a point so as not to be recognizable. This should **rarely** change.
