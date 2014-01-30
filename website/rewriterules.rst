Using Apache Rewrite Rules
==========================

The code for this blog (http://github.com/kd0kfo/blog) identifies each entry using a unique integer id. This is passed to the render script that reads the ReStructured Text and generates an html file. This is convenient for machines, but not so convenient for people.

So I used the Apache RewriteRule set below to allow descriptive paths to be used instead of numbers.

Thus, http://blog.davecoss.com/bikes/new_parts may be used instead of http://blog.davecoss.com/cgi-bin/render.cgi?id=4

Apache code:

.. code::

   RewriteEngine On
   RewriteRule ^$ index.php [L]
   RewriteRule ^categories/*$ cgi-bin/render.cgi [L]
   RewriteCond %{REQUEST_FILENAME} !-f 
   RewriteRule ^category/([^/]+)/*$ cgi-bin/render.cgi?cat=$1 [L]
   RewriteCond %{REQUEST_FILENAME} !-f 
   RewriteRule ^(.*)/*$ cgi-bin/render.cgi?id=$1 [L]
