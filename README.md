# Apache Hadoop Cluster

Automated scripts to provision a multi-node Hadoop cluster based on instructions at
https://hadoop.apache.org/docs/r2.7.2/hadoop-project-dist/hadoop-common/ClusterSetup.html.

Uses Vagrant with a simple Bash provisioner script to create Ubuntu VMs
(one master and two slaves) with the following installed:

* Ubuntu 15.10
* Apache Hadoop 2.7.2 (binary)
* Java: OpenJDK 8 (Ubuntu package)

## Creating the cluster

```
vagrant up
```

## Running the example script

Run the command below from your host machine to:

* format an HDFS filesystem on the master node;
* start HDFS, running NameNode on master and DataNode on slaves;
* start YARN, running ResourceManager on master and NodeManager on slaves;
* put input files into HDFS;
* run the 'grep' MapReduce program;
* get result files from HDFS;
* stop YARN and HDFS.

```
vagrant ssh master -- "bash -i run-example.sh"
```

## Caching

To speed up builds by caching downloaded Debian packages on your host machine,
run this first:

```
vagrant plugin install vagrant-cachier
```

Note that the provisioner script will download binary packages (e.g. Hadoop) to
the `./cache` directory on your host. This is done to avoid repeated downloads
in a similar way to how vagrant-cachier works for Linux packages. In particular,
it avoids you downloading the 200+ MB Hadoop distribution every build!
