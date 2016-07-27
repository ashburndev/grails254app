# Grails 2.1.1 Framework to Grails 2.5.4 Framework Migration

## Background

Our team has a Grails 2.1.1 web application that we want to be able to both build and deploy using Java 8.
So we are migrating that web application to the Grails 2.5.4 framework (as of this writing, Grails 2.5.4 is
the newest Grails 2.x release).

Our approach is to create an empty (new) Grails 2.5.4 web application project, and then move individual files
from the existing Grails 2.1.1 project directory to the Grails 2.5.4 project directory.

## Creating a New Grails 2.5.4 Project

Our first step, create a new Grails 2.5.4 web application project, was accomplished on a workstation running Linux
and using the commands below.  As one of our objectives of the migration is to use Java 8 for both our development
and deployment environments, I did this work using a Java 8 JDK release (from Oracle), although I could have used
Java 7 for the following.

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
    ashburndave@dphnuc:~/g2projects/grails254app$ javac -version
    javac 1.8.0_66
    ashburndave@dphnuc:~/g2projects/grails254app$ java -version
    java version "1.8.0_66"
    Java(TM) SE Runtime Environment (build 1.8.0_66-b17)
    Java HotSpot(TM) 64-Bit Server VM (build 25.66-b17, mixed mode)
    ashburndave@dphnuc:~/g2projects/grails254app$ 

## Preparing an AWS EC2 Instance for Deployment of a Grails 2.5.4 web application

I created and prepared an AWS EC2 instance to which I later deployed the above war file (using Tomcat Manager).
As one of our objectives of the migration is to use Java 8 for both our development and deployment environments,
I did this work using a Java 8 JDK release.  For now, I am content to use an OpenJDK release of Java 8, but at
some point I intend to replace the OpenJDK Java 8 with the Oracle Java 8 JDK.

       __|  __|_  )
       _|  (     /   Amazon Linux AMI
      ___|\___|___|

    https://aws.amazon.com/amazon-linux-ami/2016.03-release-notes/
    3 package(s) needed for security, out of 12 available
    Run "sudo yum update" to apply all updates.
    [ec2-user@ip-172-31-38-83 ~]$ 
    [ec2-user@ip-172-31-38-83 ~]$ uname -a
    Linux ip-172-31-38-83 4.4.11-23.53.amzn1.x86_64 #1 SMP Wed Jun 1 22:22:50 UTC 2016 x86_64 x86_64 x86_64 GNU/Linux
    [ec2-user@ip-172-31-38-83 ~]$ 
    [ec2-user@ip-172-31-38-83 ~]$ aws --version
    aws-cli/1.10.33 Python/2.7.10 Linux/4.4.11-23.53.amzn1.x86_64 botocore/1.4.23
    [ec2-user@ip-172-31-38-83 ~]$ 
    [ec2-user@ip-172-31-38-83 ~]$ java -version
    java version "1.7.0_101"
    OpenJDK Runtime Environment (amzn-2.6.6.1.67.amzn1-x86_64 u101-b00)
    OpenJDK 64-Bit Server VM (build 24.95-b01, mixed mode)
    [ec2-user@ip-172-31-38-83 ~]$ 
    [ec2-user@ip-172-31-38-83 ~]$ javac -version
    -bash: javac: command not found
    [ec2-user@ip-172-31-38-83 ~]$ jar
    -bash: jar: command not found
    [ec2-user@ip-172-31-38-83 ~]$ 
    
    sudo yum list installed | grep ava
    sudo yum list installed | grep omcat
    sudo yum list available | grep omcat
    sudo yum list available | grep [Mm][Yy][Ss][Qq][Ll]
    sudo yum list available | grep openjdk
    sudo yum list installed | grep openjdk
    sudo yum install java-1.8.0-openjdk-devel.x86_64
    sudo yum remove java-1.7.0-openjdk.x86_64
    sudo yum install tomcat7.noarch tomcat7-admin-webapps.noarch tomcat7-docs-webapp.noarch tomcat7-webapps.noarch
    sudo yum list installed | grep omcat
    sudo service tomcat7 start
    cd /usr/share/tomcat7/conf/
    sudo cp -p tomcat-users.xml tomcat-users.xml.orig
    sudo vi tomcat-users.xml
    sudo service tomcat7 stop
    sudo service tomcat7 start
    
    [ec2-user@ip-172-31-38-83 ~]$ 
    [ec2-user@ip-172-31-38-83 ~]$ java -version
    openjdk version "1.8.0_91"
    OpenJDK Runtime Environment (build 1.8.0_91-b14)
    OpenJDK 64-Bit Server VM (build 25.91-b14, mixed mode)
    [ec2-user@ip-172-31-38-83 ~]$ 
    [ec2-user@ip-172-31-38-83 ~]$ javac -version
    javac 1.8.0_91
    [ec2-user@ip-172-31-38-83 ~]$ jar
    Usage: jar {ctxui}[vfmn0PMe] [jar-file] [manifest-file] [entry-point] [-C dir] files ...

The grails254app.war has been deployed to the above AWS EC2 instance, and is available using the URL below.
At this stage this Grails 2.5.4 web application is about the simplist possible Grails 2.5.4 web application.
But it does demonstrate a basic compatiblity of the Grails 2.5.4 Framework with a Java 8 development and
deployment environment.

http://ec2-52-87-193-41.compute-1.amazonaws.com:8080/grails254/

## Adding a Domain class, a Controller and Views

The work described above was a first test to see if a Grails 2.5.4 web application could be developed and deployed using Java 8.  While it worked (and that was a significant result), a more robust Grails application is certainly needed to ascertain real compatibility with Java 8.  So I modified the grails254app project, adding a domain class and a corresponding controller and views.

    grails set-version 0.2
    grails create-domain-class com.nile.Book
    vi grails-app/domain/com/nile/Book.groovy
    grails generate-all com.nile.Book
    grails war target/grails254app.war

The first version of the simple domain class looks like this.

    package com.nile
    
    class Book {
        String title
        String author
        String comment
        static constraints = {
          title maxSize: 50, blank: false
          author maxSize: 35, nullable: true
          comment maxSize: 1000, nullable: true
        }
    }

I again used the Tomcat manager to deploy the war file to my AWS EC2 instance, and the new version of the web application is now running and available at the same URL as before.

http://ec2-52-87-193-41.compute-1.amazonaws.com:8080/grails254/















