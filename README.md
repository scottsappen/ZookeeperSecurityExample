# ZookeeperSecurityExample

This is a simple example of working with current ZooKeeper security

This is important because ZK allow any connected user all privileges. Kafka stores a lot in ZK, including:
- broker topics
- broker ids
- topic configurations
- ACLs

ZK has 3 ACL schemes
- "world" which is the default and means no authentication
- "digest" which is a username/password combination
- "SASL" for kerberos authentication

We want to make it such that Kafka has cdwra (create, delete, write, read, admin) and "world" only has r (read).

Alternatively (and much simpler) is to simply use network rules to only allow Kafka and specific clients/servers to communicate with ZK.

If you want to set up your own Kerberos server to generate your own keytab files, you can reference the repo MITKerberosExample - https://github.com/scottsappen/MITKerberosExample.

Also, in this example, we will run zookeeper on a simple ubuntu box along with Kafka and krb5-user utilities. If you want to follow a similar path, you can look at the repo KafkaSSLAuthExample (Part 1) - https://github.com/scottsappen/KafkaSSLAuthExample

**What we will do**

- Enable security on each ZK node
- Configure brokers to use same security credentials to access ZK
- Configure ZK to use "secure znode" by default

Reference the various sub-directories for the following:
- Part 1 - Creating your principals and keytabs for ZK
- Part 2 - Configuring SASL in ZK
- Part 3 - 2DO: Enabling ZK-authorization
- Part 4 - 2DO: Migrating znodes to secure znodes
