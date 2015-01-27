#REST Proxy Example

##Requirements

* Apache Maven 3.x ([http://maven.apache.org](http://maven.apache.org))
* JBoss Fuse 6.1.x ([http://jboss.org/jbossfuse](http://jboss.org/jbossfuse))
* JBoss EAP 6.3.x ([http://www.jboss.org/products/eap](http://www.jboss.org/products/eap))

##Building Example

```Shell
$ cd <POC_HOME>
$ mvn clean install
```

## Running the example (standalone)

Start the server:

```Shell
$ cd <JBOSS_FUSE_HOME>/bin
$ ./fuse
```

From the JBoss Fuse console, enter the following to add the users:

```
JBossFuse:karaf@root> esb:create-admin-user --new-user admin --new-user-password admin --new-user-role admin
```

From the JBoss Fuse console, enter the following to install the example application:

```
JBossFuse:karaf@root> features:install camel-core
JBossFuse:karaf@root> features:install camel-blueprint
JBossFuse:karaf@root> features:install camel-cxf
JBossFuse:karaf@root> features:install activemq-camel
JBossFuse:karaf@root> features:install activemq-client
JBossFuse:karaf@root> osgi:install -s mvn:org.jboss.examples/greeter-api/1.0.0-SNAPSHOT
JBossFuse:karaf@root> osgi:install -s mvn:org.jboss.examples/greeter-backend/1.0.0-SNAPSHOT
JBossFuse:karaf@root> osgi:install -s mvn:org.jboss.examples/greeter-frontend/1.0.0-SNAPSHOT
```

## Running the example (fabric)

Start the server:

```Shell
$ cd <JBOSS_FUSE_HOME>/bin
$ ./fuse
```

From the JBoss Fuse console, enter the following to add the users:

```
JBossFuse:karaf@root> esb:create-admin-user --new-user admin --new-user-password admin --new-user-role admin
```

From the JBoss Fuse console, enter the following to create a fabric:

```
JBossFuse:karaf@root> fabric:create --clean --wait-for-provisioning --profile fabric --global-resolver manualip --manual-ip 127.0.0.1 root
```

From the command prompt, enter the following install the fabric profiles:

```Shell
$ cd <POC_HOME>/greeter-backend
$ mvn fabric8:deploy
$ cd <POC_HOME>/greeter-frontend
$ mvn fabric8:deploy
```

From the JBoss Fuse console, enter the following to create the necessary containers and apply appropriate profiles:

```
JBossFuse:karaf@root> fabric:mq-create --parent-profile mq-base --group default --kind StandAlone --minimumInstances 1 --create-container mq mq1
JBossFuse:karaf@root> fabric:container-create-child --profile rsproxy-backend root be 2
JBossFuse:karaf@root> fabric:container-create-child --profile rsproxy-frontend root fe1 1
```
