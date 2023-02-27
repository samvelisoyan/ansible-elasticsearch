Elastic Search
=========

An Ansible Role that installs and configures Elastic Search
This service can be managed by servicetools resource


Requirements
------------
### Operating Systems support

* Centos 7
* Redhat 7,8


Dependencies
------------

This role automatically can install JAVA (java-11-openjdk)
By default, the systemd services is managed servicetools through(requires servicetools role dependency)
You(with good rights) can use 'users' role as a dependency, when it is required to create user accounts used by application (like 'elasticsearch' user)


Variables and Properties
------------------------

### Role Variables

| Variables | Required | Default value | Description |
|-----------|----------|---------------|-------------|
| elasticsearch_version | False | *7.x* | Major version of elasticsearch package from elasticsearch repository |
| elasticsearch_disable_install | False | *false* | Will skip packages installation, service managed and directories creation. |
| elasticsearch_enabled_on_boot | False | *true* | enabled systemd service on boot(as root user) |
| elasticsearch_create_config | False | *true* | Whether to create the elasticsearch configuration file |
| elasticsearch_disable_java_install | False | *false* | Will skip java packages installation, and will use bundled version of OpenJDK |
| elasticsearch_app_java_package | False | *java-11-openjdk* | Java pakage name which must be installed. check supported versions(https://www.elastic.co/support/matrix#matrix_jvm) |
| elasticsearch_disable_remove_plugin | False | *true* |  Disable Remove of not listed elasticsearch plugins |
| elasticsearch_install_plugins | False | *[]* | List of Elasticsearch plugins that should be installed |
| elasticsearch_user | False | *elasticsearch* | Owner of Elasticsearch configuration files on server |
| elasticsearch_group | False | *elasticsearch* | Group owning Elasticsearch configuration files on server |
| elasticsearch_jvm_options_heap_size | False | *2g* | Allows to mention Java Heap size |
| elasticsearch_jvm_custm_options | False | *[]* | Allows to add custm jvm options |
| elasticsearch_listen_address | False | *0.0.0.0* | The node will bind to this address and will also use it as its publish address. Accepts an IP address, a hostname, or a special value (https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-network.html) for more info.
| elasticsearch_http_port | False | *9200* | The port to bind for HTTP client communication. Accepts a single value or a range. If a range is specified, the node will bind to the first available port in the range(https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-network.html) for more info.
| elasticsearch_log_dir | False | */var/log/elasticsearch* | log directory path on server(as root user) |
| elasticsearch_data_dir | False | */var/lib/elasticsearch* | Allows to mention where to store Data(as root user) |
| elasticsearch_disable_action_auto_create_index | False | *true* | allows any index to be created automatically(https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-index_.html) for more info |
| elasticsearch_config | False | *{}* | You can add your ElasticSearch configuration which will be added to elasticsearch.yml |
| elasticsearch_enable_cluster | False | *false* | allow configure elasticsearch cluster. Next configs will be mandatory |
| elasticsearch_cluster_name | False | *""* | Use a descriptive name for your cluster |
| elasticsearch_node_name | False | *""* | Use a descriptive name for the node |
| elasticsearch_discovery_seed_hosts | False | *""* | Pass an initial list of hosts to perform discovery when this node is started: The default list of hosts is ["127.0.0.1", "[::1]"] |
| elasticsearch_cluster_initial_master_nodes | False | *""* | Bootstrap the cluster using an initial set of master-eligible nodes |
| elasticsearch_discovery_zen_no_master_block | False | *""* | controls what operations should be rejected when there is no active master(https://www.elastic.co/guide/en/elasticsearch/reference/6.8/modules-discovery-zen.html) for more info.
| elasticsearch_discovery_zen_minimum_master_nodes | False | *""* | sets the minimum number of master eligible nodes that need to join a newly elected master in order for an election to complete and for the elected node to accept its mastership(https://www.elastic.co/guide/en/elasticsearch/reference/6.8/modules-discovery-zen.html) for more info. |
| elasticsearch_disable_service_managed_by_servicetools | False | *false* | systemd service can be managed by servicetools systemd resource(as root user) |
| elasticsearch_disable_auto_started_by_servicetools | False | *false* | systemd service can be automatically started or re-started by servicetools(as root user) |
| elasticsearch_custom_configuration | False | *[]* | Helps to deploy multiple nodes to the same server |

#### elasticsearch custom configuration

Please note that all Required variables must be uniq for the host
Each elasticsearch custom configuration files will be deployed under /etc/elasticsearch/es[ instance number ]

| Variables | Required | Default value | Description |
|-----------|----------|---------------|-------------|
| elasticsearch_jvm_options_heap_size | False | *""* | Allows to mention Java Heap size |
| elasticsearch_jvm_custm_options | False | *[]* | Allows to add custm jvm options |
| elasticsearch_listen_address | true | *""* | The node will bind to this address and will also use it as its publish address. Accepts an IP address, a hostname, or a special value. |
| elasticsearch_http_port | true | *""* | The port to bind for HTTP client communication. Accepts a single value or a range. If a range is specified, the node will bind to the first available port in the range. |
| elasticsearch_log_dir | true | *""* | Allows to mention where to store logs |
| elasticsearch_data_dir | true | *""* | Allows to mention where to store Data |
| elasticsearch_disable_action_auto_create_index | False | *""* | allows to disable index automatic creation, default is false |
| elasticsearch_config | False | *""* | You can add your custm ElasticSearch configurations which will be added to elasticsearch.yml file content |
| elasticsearch_enable_cluster | false | *""* | allow configure elasticsearch cluster. Next configs will be mandatory |
| elasticsearch_cluster_name | false | *""* | Use a descriptive name for your cluster |
| elasticsearch_node_name | False | *""* | Use a descriptive name for the node |
| elasticsearch_discovery_seed_hosts | False | *""* | Pass an initial list of hosts to perform discovery when this node is started: The default list of hosts is ["127.0.0.1", "[::1]"] |
| elasticsearch_cluster_initial_master_nodes | False | *""* | Bootstrap the cluster using an initial set of master-eligible nodes |
| elasticsearch_discovery_zen_no_master_block | False | *""* | controls what operations should be rejected when there is no active master. |
| elasticsearch_discovery_zen_minimum_master_nodes | False | *""* | sets the minimum number of master eligible nodes that need to join a newly elected master in order for an election to complete and for the elected node to accept its mastership. |
| elasticsearch_service_name | true | *""* | ElasticSearch service name which will be used to configure elasticsearch service in servicetools and systemd |

### Role Vars file

By example, I would like to install(as root user) elasticsearch and set configuration files, place following content into vars yaml file

```yaml
---
# Installation
elasticsearch_version: '7.x'
elasticsearch_disable_install: false
elasticsearch_enabled_on_boot: true
elasticsearch_create_config: true
elasticsearch_disable_java_install: false
elasticsearch_app_java_package: 'java-11-openjdk'
elasticsearch_disable_remove_plugin: true
elasticsearch_install_plugins: []

# configuration
elasticsearch_user: elasticsearch
elasticsearch_group: elasticsearch

elasticsearch_jvm_options_heap_size: 2g

elasticsearch_listen_address: 0.0.0.0
elasticsearch_http_port: 9200
elasticsearch_log_dir: /var/log/elasticsearch
elasticsearch_data_dir: /var/lib/elasticsearch
elasticsearch_disable_action_auto_create_index: false
elasticsearch_config: {}

#elasticsearch cluster
elasticsearch_enable_cluster: false
elasticsearch_cluster_name: ""
elasticsearch_node_name: ""
elasticsearch_discovery_seed_hosts: ""
elasticsearch_cluster_initial_master_nodes: ""
elasticsearch_discovery_zen_no_master_block: ""
elasticsearch_discovery_zen_minimum_master_nodes: ""

elasticsearch_custom_configuration:
  - elasticsearch_jvm_options_heap_size: 4g
    elasticsearch_listen_address: 0.0.0.0
    elasticsearch_http_port: 9201
    elasticsearch_log_dir: /var/log/elasticsearch1
    elasticsearch_data_dir: /var/lib/elasticsearch1
    elasticsearch_disable_action_auto_create_index: false
    elasticsearch_config: {}
    elasticsearch_service_name: elasticsearch1
  - elasticsearch_jvm_options_heap_size: 2g
    elasticsearch_listen_address: 0.0.0.0
    elasticsearch_http_port: 9202
    elasticsearch_log_dir: /var/log/elasticsearch2
    elasticsearch_data_dir: /var/lib/elasticsearch2
    elasticsearch_disable_action_auto_create_index: false
    elasticsearch_config: {}
    elasticsearch_enable_cluster: false
    elasticsearch_cluster_name: ""
    elasticsearch_node_name: ""
    elasticsearch_discovery_seed_hosts: ""
    elasticsearch_cluster_initial_master_nodes: ""
    elasticsearch_discovery_zen_no_master_block: ""
    elasticsearch_discovery_zen_minimum_master_nodes: ""
    elasticsearch_service_name: elasticsearch2

# systemd service can be managed servicetools through (require servicetools role dependency)
elasticsearch_disable_service_managed_by_servicetools: false

# disabling service is started on change
elasticsearch_disable_auto_started_by_servicetools: false
elasticsearch_servicetools_customer: elasticsearch
elasticsearch_servicetools_service_name: elasticsearch
```

Example Playbook
----------------

Including an example of how to use your role :
```yaml
---
    - hosts: servers
      vars_files:
        - vars_file.yml
      roles:
        - role: elasticsearch
```

License
-------

BSD

Author Information
------------------
samvelisoyan@hotmalil.com
