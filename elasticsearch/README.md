# Ansible Playbook for Elasticsearch

This is an [Ansible](http://www.ansibleworks.com/) playbook for [Elasticsearch](http://www.elasticsearch.org/). You can use it by itself or as part of a larger playbook customized for your local environment.

## Features
- Support for installing plugins
- Support for installing and configuring EC2 plugin
- Support for installing custom JARs in the Elasticsearch classpath (e.g. custom Lucene Similarity JAR)
- Support for installing the [Sematext SPM](http://www.sematext.com/spm/) monitor
- Support for installing the [Marvel](http://www.elasticsearch.org/guide/en/marvel/current/) plugin


## Running Standalone Playbook


### Add your my-inventory.ini
Edit your my-inventory.ini and customize your cluster and node names:

```
#########################
# Elasticsearch Cluster #
#########################
[es_node_1]
1.2.3.4.compute-1.amazonaws.com
[es_node_1:vars]
elasticsearch_node_name=elasticsearch-1

[es_node_2]
5.6.7.8.compute-1.amazonaws.com
[es_node_2:vars]
elasticsearch_node_name=elasticsearch-2

[es_node_3]
9.10.11.12.compute-1.amazonaws.com
[es_node_3:vars]
elasticsearch_node_name=elasticsearch-3

[all_nodes:children]
es_node_1
es_node_2
es_node_3

[all_nodes:vars]
elasticsearch_cluster_name=my.elasticsearch.cluster
elasticsearch_plugin_aws_ec2_groups=MyElasticSearchGroup
spm_client_token=<your SPM token here>
```

### Edit your vars/my-vars.yml
See `vars/sample.yml` for example variable files. These are the files where you specify Elasticsearch settings and apply certain features such as plugins, custom JARs or monitoring. The best way to enable configurations is to look at `templates/elasticsearch.yml.j2` and see which variables you want to defile in your `vars/my-vars.yml`. See below for configurations regarding EC2, plugins and custom JARs.

### Edit your my-playbook-main.yml
Example `main.yml`:

```
---

#########################
# Elasticsearch install #
#########################

- hosts: all_nodes
  user: $user
  sudo: yes

  vars_files:
    - defaults/main.yml
    - vars/my-vars.yml

  tasks:
    - include: tasks/main.yml
```

### Launch
```
$  ansible-playbook main.yml -i my-inventory.ini -e user=<your sudo user for the elasticsearch installation>
```
