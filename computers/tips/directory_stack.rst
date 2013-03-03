Directory Stack. Pure Awesome.
===============================

Started using *dirs* and wish I had started earlier. I often switch between quite a few directories. For example, if I am testing a new program feature, I might be in the build directory and doing multiple tests in different directories, for organizational reasons (I save everything). Screen helps this, but I like to have one screen for all of the tests. Dirs makes this easier.


* dirs -- lists directories stored in the stack. Good options: -l, full path resolution. +/-N shows only the Nth item on the stack from the top/bottom. Using this in conjunction with cd by enclosing dirs in accent marks is super slick.

* pushd -- adds the specified directory to the stack and switches the current working directory to that new directory. To avoid that directory switch, use the "-n" option.

* popd -- pops the stack and switches to that directory. Good options: +/-N pops the Nth directory. -n pops without switching directories, effectively just removing and entry from the stack.

http://www.faqs.org/docs/bashman/bashref_73.html
