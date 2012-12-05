A quickstart openxava-4.6 project, MySchool, built by maven
===========================================================

There is not an attempt to satisfy a build for every container, database server in the world.  Currently,
this output is build to run on a tomcat 6.0 container, hsqldb server as configured as in the
openxava distribution.  It can serve as a quickstart maven project for openxava.

Environment recommendations:
---------------------------
Eclipse 4.2 (Juno) SR1 for JEE developers.
Hibernate target is 4.x.
Tomcat 6.x (as configured in the openxava distribution)

General steps:
--------------

1. Put openxava in your local repository,
2. set your mvn repositories (requires acces to jboss maven repository)**
3. installing eclipse for jee developers, and plugins: wtp, m2e.
4. import this project to eclipse
5. create a tomcat server in eclipse and add this project
6. debug and code and debug and code.

**This maven build depends on jboss provided api jar files.  You can change
this project to use a different set of api jar files.


#Put openxava in your local repository:
openxava is not available on maven central.  You must install it manually to your local repository.

For the jar binary, unpack the download distribution,
openxava.jar
    # version.properties file inside the openxava.jar archive specifies the version
    jar xf openxava.jar version.properties; cat version.properties
  
    # use the version number from that file in the version field below
    mvn install:install-file -DgroupId=org.openxava -DartifactId=openxava -Dversion=4.6 -Dpackaging=jar  -Dfile=lib/openxava.jar
    mvn install:install-file -DgroupId=org.openxava -DartifactId=ox-jdbc-adapters -Dversion=4.6 -Dpackaging=jar -Dfile=lib/ox-jdbc-adapters.jar
  
Eclipse supports source code debugging, and it can be helpful if you want to watch openxava work, however,
openxava source jar file must be prepared and installed to your local maven repository by hand:

    # checkout code from svn server
    svn list svn://svn.code.sf.net/p/openxava/code/tags
    svn checkout svn://svn.code.sf.net/p/openxava/code/tags/OpenXava_20121122_v4_6/OpenXava/src
    jar -cf openxava-source.jar -C src .
    mvn install:install-file -DgroupId=org.openxava -DartifactId=openxava -Dversion=4.6 -Dpackaging=jar -Dclassifier=sources  -Dfile=openxava-source.jar

    jar -cf ox-jdbc-adapters-source.jar -C notcompile .
    mvn install:install-file -DgroupId=org.openxava -DartifactId=ox-jdbc-adapters -Dversion=4.6 -Dpackaging=jar -Dclassifier=sources  -Dfile=ox-jdbc-adapters-source.jar
  


To import to eclipse:
---------------------
1. clone this project
2. use eclipse import maven project and import this project
3. set dynamic web version in project facet to match your server.  For tomcat 6.0, the dynamic web version is 2.3.


Configuring tomcat
------------------

The tomcat in openxava distribution is version 6.0.20 (2008), currently available is 6.0.36.
If you want to use a fresher version
1. copy the datasource defininitions from openxava distribution's tomcat/conf/context.xml
2. copy some jar files from openxava distribution's tomcat/lib: ejb.jar, jta.jar, jasper-jdt.jar


openxava's librariy dependencies
-----------------------

For all dependencies, I discovered versions by inspecting all the jars in the openxava
distribution.  I used the same versions in this build.  However, I have a requirement to use Hibernate 4.x; openxava
uses Hibernate 3.6.x.


###library inclusions.

Aside from the obvious (openxava, hibernate, hsqld, logger), here are some archives that
were carried over from the openxava distributions 's MySchool build

* ox-jdbc-adapters


###library exclusions.

These libraries are not dependencies, neither runtime nor compile time, of MySchool quickstart demo, and
are excluded from this builid.

* activation:  this is included in java 1.6+ and jee 5+.  http://www.oracle.com/technetwork/java/jaf11-139815.html

* cglib, asm: as of hibernate 3.5, cglib is replaced by javassist.  The hibernate 4.x transitive dependencies
pulls in javassist.  http://jaxenter.com/hibernate-to-deprecate-cglib-as-bytecode-provider-30595.html

* autobizlogic, commons-jexl, groovy-all: myschool quick start does not require autobizlogic.
Note, autobizlogic is available on maven repository: http://www.automatedbusinesslogic.com/reference/using-maven

* jaxb: this is included in java 1.6+.

* dsn, imap, mailapi, pop3: myschool quick start does not require email support.
Note: these are bundled in JavaMail from oracle.  JavaMail is included in java 1.6+ and jee.

* ehcache: myschool quick start does not require ehcache.

* jakarta-oro:  This apache project was mothballed in 2009.   Which code is dependent on this: apparently, none.

* poi: openxava generates csv files, not xsl files (see org.openxava.web.servlets.GenerateCustomReportServlet),
and does not reference this artifact.

* commons-validator: myschool quick start does not use the xava validators that depend on this archive.
Use bean validation because commons-validator has not a release since 2006 (this is 2012).

* jpa2: Instead of using this java.persistence api library of unknown origin, maven 
downloads a published versioned library from a known site, for example jboss (home of hibernate).

* commons-io: used by htmlunit, needed in production?  These are not compile time dependencies
and are not brought in by maven.


TODO
----
* TODO: fix reports (not working in this conversion)
* TODO: dtds are in openxava.jar and should not also be in the project's webapp directory.
* TODO: if dtds must be in this project's webapp, they eclipse fail validation and should be fixed.














