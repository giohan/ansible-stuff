# Ansible Playbook: cloudera-hadoop

An ansible playbook to deploy Cloudera hadoop components to the cluster
# Overview
The playbook is composed according to [official cloudera guides](http://www.cloudera.com/content/www/en-us/documentation/enterprise/5-4-x/topics/cdh_ig_command_line.html) with a primary purpose of production deployment in mind. High availability for **HDFS** and **Yarn** is implemented when a sufficient number of resources(hosts) is configured. From the other side, all of the components can be also deployed on a single host.

# Description
#### The playbook is able to setup the required services for components:
* hadoop hdfs
* hadoop yarn mapreduce
* zookeeper
* hive
* hbase
* impala
* solr
* spark
* oozie
* kafka
* hue
* postgresql

The configuration is _very_ simple:

It’s only required to place hostname(s) to the appropriate group in the [hosts](hosts) file, and the required services will be setup.

The playbook contain all configuration files in roles directories. If you need to add or change any parameter you can edit
the required configuration file which can be found in roles/_service_/[files|templates] directory.

The playbook runs configuration check tasks at start, and will stop if the configuration is not supported,
providing a descriptive error message.

# Configuration
Service configuration performed using the hosts file. The empty [hosts](https://github.com/sergevs/ansible-cloudera-hadoop/blob/master/hosts) file is supplied with playbook. **You must not remove any existing group**. Leave the group empty if you don't need services the group configures. The same hostname can be placed to any hosts group. As an instance if you want setup everything on one host, just put the same hostname to each hosts group.


# Usage
To start deployment run:

    ansible-playbook -i hosts site.yaml


To deploy configuration on existing cluster:

    ansible-playbook -i hosts --skip-tags=init,postgresql site.yaml

#### Tags used in playbook:
* **package** : install rpm packages
* **init** : clean up and initialize data
* **config** : deploy configuration files, useful if you want just change configuration on hosts.
* **test** : run test actions
* **check** : check hosts configuration


# Requirements
[Ansible >= 2.2.0.0](http://www.ansible.com) is required. Please read [official documentation](http://docs.ansible.com/ansible/intro_installation.html#latest-release-via-yum) to install it.

OS version: Redhat/CentOS 6, 7

Cloudera Hadoop version: 5.4 - 5.9

The required for Cloudera Hadoop repositories have to be properly configured on the target hosts.
See also [official documentation](http://www.cloudera.com/content/www/en-us/documentation/enterprise/latest/topics/cdh_ig_yumrepo_local_create.html)

Java package(s) have to be available in the repository. You can download [jdk-8u65-linux-x64.rpm](http://www.oracle.com/technetwork/java/javase/downloads/java-archive-javase8-2177648.html#jdk-8u60-oth-JPR) from official oracle site

SSH key passwordless authentication must be configured for root account for all target hosts.

**se_linux** must be disabled

**remote_user = root** must be configured for ansible.

