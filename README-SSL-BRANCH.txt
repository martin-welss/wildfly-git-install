This branch adds ssl support.

The goal is to let each system (development, test, production) have its 
own ssl certificate which is not checked into version control. This
is especially important for the sensitive production certificate.
The location of the keystore is the same for all systems, but the
password is configured by the following system.property:

ssl.keystore.password


Please copy the file server.keystore.sample to $HOME/server.keystore
where $HOME ist the home directory of the user running wildfly.

