# ZookeeperSecurityExample

**Configuring SASL in ZK**

Edit your zookeeper configuration file

```
sudo vi ../etc/kafka/zookeeper.properties

authProvider.1=org.apache.zookeeper.server.auth.SASLAuthenticationProvider
jaasLoginRenew=3600000
```

Create your JaaS file for ZooKeeper

```
sudo vi ../etc/kafka/zookeeper_jaas.conf

Server {
com.sun.security.auth.module.Krb5LoginModule required
useKeyTab=true
storeKey=true
keyTab="/tmp/zookeeper.service.keytab"
principal="zookeeper/<your kafka internal DNS server>@KAFKA.SECURE";
};
```

Edit the kafka broker jaas file. The section Client may look confusing, but see it from ZK's perspective. Kafka broker is acting as a client to ZK so it makes sense to call it Client.

```
sudo vi ../etc/kafka/kafka_server_jaas.conf

Client {
    com.sun.security.auth.module.Krb5LoginModule required
    useKeyTab=true
    storeKey=true
    keyTab="/tmp/zookeeper.service.keytab"
    principal="ubuntu/<your kafka server>@KAFKA.SECURE";
};
```

Let's start zookeeper and Kafka with the new settings.

Now if you were using systemd, you would modify the systemd scripts. In this example, we will pass it in command line.

```
KAFKA_OPTS="-Djava.security.auth.login.config=/home/ubuntu/confluent-5.1.1/etc/kafka/zookeeper_jaas.conf" ./confluent start zookeeper

KAFKA_OPTS="-Djava.security.auth.login.config=/home/ubuntu/confluent-5.1.1/etc/kafka/kafka_server_jaas.conf -Dsun.security.krb5.debug=true" ./confluent start kafka
```
