# ZookeeperSecurityExample

**Create our principals and keytab files**

Connect to your kerberos server

```
ssh -i <your pem file> centos@<your kerberos server>
```

Create a principal. In this case, we have zookeeper running as user ubuntu.

Note: make sure you specify the internal DNS name of the kafka broker here to ensure the forward and reverse DNS lookups provide matching values. If you provide the external address, then reverse DNS lookups will not return correctly.

```
sudo kadmin.local -q "add_principal -randkey ubuntu/ip-172-31-1-177.us-east-2.compute.internal@KAFKA.SECURE"
```

Now export those principals into a zookeeper keytab file.
```
sudo kadmin.local -q "xst -kt /tmp/zookeeper.service.keytab ubuntu/ip-172-31-1-177.us-east-2.compute.internal@KAFKA.SECURE"
```

Download these keytab files to your zookeeper server.

In this example, we will copy it to a local laptop from the kerberos server and then back up to the zookeeper server. In the real world again, you might want to put those somewhere other than /tmp so they survive a reboot.

```
sudo chmod a+r /tmp/*.keytab

Copy to local machine
scp -i ~<your pem file>.pem centos@<your kerberos server>:/tmp/*.keytab /tmp/

Upload to kerberos user server
scp -i ~<your pem file>.pem *.keytab ubuntu@<your zookeeper server>:/tmp/
```

Now login to your zookeeper server.

Let's test by grabbing a ticket for the admin principal using the admin keytab.
```
kinit -kt /tmp/zookeeper.service.keytab ubuntu/ip-172-31-1-177.us-east-2.compute.internal

klist
```
