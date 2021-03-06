Wildfly-git-install
===================

wildfly-git-install lets you install your customized wildfly-installation on all your computers and servers of your build-pipeline: development workstations, testsystems, ci-servers and production systems. Just setup a central git-repository and change the configuration to your needs and clone it on the other servers.

The system-specific configurations like database url, user and password are externalized to the file system.properties residing in the users home directory. Of course the local system.properties are NOT tracked by git. By using git to manage your installations you get all its  power and benefits you are used to:
- easily track changes and nail down configuration differences or bugs
- switch back and forth between commits and branches
- experiment with new configurations without regrets

A sample system.properties may look like this:

    exampleds.jdbc.url: jdbc:postgresql:example
    exampleds.user: wildfly
    exampleds.password: wildfly    
    txn.node.identifier: node-test1
    jboss.bind.address: localhost

And you start wildfly like this:

    ./bin/standalone.sh --server-config=standalone-full.xml -P=$HOME/system.properties

The respective section in standalone-full.xml looks like this:

    <datasource jndi-name="java:jboss/datasources/ExampleDS" pool-name="ExampleDS" enabled="true" use-java-context="true">
       <connection-url>${exampleds.jdbc.url}</connection-url>
       <driver>postgres</driver>
       <security>
          <user-name>${exampleds.user}</user-name>
          <password>${exampleds.password}</password>
       </security>
   

Local logs and pure runtime files are ignored, so here is the content of .gitignore
 
    standalone/configuration/standalone_xml_history
    standalone/data
    standalone/deployments
    standalone/log
    standalone/tmp   


Beyond system.properties
========================

Not all configurations can be handled by system.properties. If you need debugging, you just add the option --debug to your
commandline:

    ./bin/standalone.sh --debug --server-config=standalone-full.xml -P=$HOME/system.properties

To adjust JVM options like max heapspace -Xmx just set $JAVA_OPTS accordingly in your environment.

IMPORTANT: 
Using jboss-cli for deployment adds the deployment to the respective configuration file (e.g. standalone-full.xml), so
this method is NOT suited for our needs. But there is another way: We should use drop-in deployment and monitor the marker files.
Just look at the file build.gradle in the companion project 

https://github.com/martin-welss/jtrack-ee7

Database
========

This configuration needs a valid login to a PostgreSQL 9.4 Database (or a previous version), like shown in the sample system.properties.




