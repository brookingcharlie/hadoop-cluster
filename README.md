# Apache Hadoop Cluster

Automated scripts to provision a multi-node Hadoop cluster based on instructions at
https://hadoop.apache.org/docs/r2.7.2/hadoop-project-dist/hadoop-common/ClusterSetup.html.

Uses Vagrant with a simple Bash provisioner script to create two Ubuntu VMs
(master and slave) with the following installed:

* Ubuntu 15.10
* Apache Hadoop 2.7.2 (binary)
* Java: OpenJDK 8 (Ubuntu package)

## Caching

To speed up builds by caching downloaded Debian packages on your host machine,
run this first:

    vagrant plugin install vagrant-cachier

Note that the provisioner script will download binary packages (e.g. Hadoop) to
the `./cache` directory on your host. This is done to avoid repeated downloads
in a similar way to how vagrant-cachier works for Linux packages. In particular,
it avoids you downloading the 200+ MB Hadoop distribution every build!
