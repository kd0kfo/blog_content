New Maven Project
=================

I used this command to create a maven project file, pom.xml, for my webfs core code. I like interactiveMode=true, because I can review the information and manually set the versio number.

.. code:: bash

   $ mvn archetype:generate -DgroupId=com.davecoss.webfs -DartifactId=webfscore -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=true
