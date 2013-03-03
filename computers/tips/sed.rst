sed to the rescue
==================

I had to replace a string in a lot of files. This command came in handy.

     sed -e 's/old text/new test/g' original.txt > new.txt

I then put in a loop (using bash, of course)

     for i in `grep "old text" * -l`;do sed -e 's/old text/new test/g' $i > new.txt;mv new.txt $i;done

Slick way of edit a large number of files.
