Our team has a Grailss 2.1.1 web application that we want to be able to both build and deploy using Java 8.
So we are migrating that web application to the Grails 2.5.4 framework (as of this writing, Grails 2.5.4 is
the newest Grails 2.x release).

Our approach is to create an empty (new) Grails 2.5.4 web application project, and then move individual files
from the existing Grails 2.1.1 project directory to the Grails 2.5.4 project directory.

Our first step, create a new Grails 2.5.4 web application project, was accomplished on a workstation running Linux
and using the commands below.

    grails create-app grails254app
    cd grails254app/
    git init
    git add *
    git add .classpath .project 
    grails integrate-with --git
    git add .gitignore 
    git commit -m "first commit"
    git remote add origin https://github.com/ashburndev/grails254app.git
    git push -u origin master
    grails war target/grails254app.war
    git add web-app/WEB-INF/tld/c.tld web-app/WEB-INF/tld/fmt.tld 
    git commit -m "commit after creating first war"
    git push

    ashburndave@dphnuc:~/g2projects/grails254app$ grails --version
    Grails version: 2.5.4
    shburndave@dphnuc:~/g2projects/grails254app$ javac -version
    javac 1.8.0_66
    ashburndave@dphnuc:~/g2projects/grails254app$ java -version
    java version "1.8.0_66"
    Java(TM) SE Runtime Environment (build 1.8.0_66-b17)
    Java HotSpot(TM) 64-Bit Server VM (build 25.66-b17, mixed mode)
    ashburndave@dphnuc:~/g2projects/grails254app$ 

