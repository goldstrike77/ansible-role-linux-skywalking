![](https://img.shields.io/badge/Ansible-skywalking-green.svg?logo=angular&style=for-the-badge)

>__Please note that the original design goal of this role was more concerned with the initial installation and bootstrapping environment, which currently does not involve performing continuous maintenance, and therefore are only suitable for testing and development purposes,  should not be used in production environments.__

>__请注意，此角色的最初设计目标更关注初始安装和引导环境，目前不涉及执行连续维护，因此仅适用于测试和开发目的，不应在生产环境中使用。__
___

<p><img src="https://raw.githubusercontent.com/goldstrike77/goldstrike77.github.io/master/img/logo/logo_skywalking.svg" height="90px" align="right" /></p>

__Table of Contents__

- [Overview](#overview)
- [Requirements](#requirements)
  * [Operating systems](#operating-systems)
  * [Skywalking Versions](#skywalking-versions)
- [ Role variables](#Role-variables)
  * [Main Configuration](#Main-parameters)
  * [Other Configuration](#Other-parameters)
- [Dependencies](#dependencies)
- [Example Playbook](#example-playbook)
  * [Hosts inventory file](#Hosts-inventory-file)
  * [Vars in role configuration](#vars-in-role-configuration)
  * [Combination of group vars and playbook](#combination-of-group-vars-and-playbook)
- [License](#license)
- [Author Information](#author-information)
- [Contributors](#Contributors)

## Overview
Apache SkyWalking is an open source APM system, including monitoring, tracing, diagnosing capabilities for distributed system in Cloud Native architecture, Especially designed for microservices, cloud native and container-based (Docker, Kubernetes, Mesos) architectures.

### Instructions
<p><img src="https://raw.githubusercontent.com/goldstrike77/goldstrike77.github.io/master/img/skywalking-illustration.jpg" /></p>

## Requirements
### Operating systems
This Ansible role installs skywalking on linux operating system, including establishing a filesystem structure and server configuration with some common operational features. Only cluster management of ZooKeeper and Elasticsearch backend storage is supported.

This role will work on the following operating systems:

  * CentOS 7

### Skywalking versions

The following list of supported the Skywalking releases:

* skywalking 7.0.0

## Role variables
### Main parameters #
There are some variables in defaults/main.yml which can (Or needs to) be overridden:
##### General parameters
* `skywalking_cluster`: Cluster name of servers that implements distribution performance.
* `skywalking_path`: This directory is used to store Apache SkyWalking server state.
* `skywalking_version`: Specify the Apache SkyWalking version.
* `skywalking_oap_jvm_xmx`: Size of the OAP service heap in MB.
* `skywalking_web_jvm_xmx`: Size of the Web UI heap in MB.

##### Storage Data TTL setting
* `skywalking_datattl.record`: Unit is day, affects Record data, including tracing and alarm.
* `skywalking_datattl.otherMetrics`: Unit is day, affects minute/hour/day dimensions of metrics.
* `skywalking_datattl.monthMetrics`: Unit is month, affects month dimension of metrics.

##### Role dependencies
* `skywalking_elastic_dept`: A boolean value, whether ElasticSearch use the same environment.
* `skywalking_zoo_dept`: A boolean value, whether ZooKeeper use the same environment.

##### Listen port
* `skywalking_port_arg`: Defines communication port.

##### Elastic parameters
* `skywalking_elastic_auth`: A boolean value, Enable or Disable Elasticsearch authentication.
* `skywalking_elastic_cluster`: Specify name for your Elastic cluster name.
* `skywalking_elastic_hosts`: List of Elasticsearch hosts Apache SkyWalking should connect to.
* `skywalking_elastic_pass`: Elasticsearch authenticated password.
* `skywalking_elastic_path`: Specify the ElasticSearch data directory.
* `skywalking_elastic_port_rest`: Elasticsearch REST port.
* `skywalking_elastic_user`: Elasticsearch authenticated user.
* `skywalking_elastic_version`: Specify the Elasticsearch version.
* `skywalking_elastic_heap_size`: Specify the maximum memory allocation pool for a Java virtual machine.

##### ZooKeeper parameters
* `skywalking_zoo_version`: Specify the ZooKeeper version.
* `skywalking_zoo_cluster`: Name of ZooKeeper cluster.
* `skywalking_zoo_path`: Specify the ZooKeeper working directory.
* `skywalking_zoo_servers`: List of ZooKeeper servers.
* `skywalking_zoo_jvm_xmx`: Size of the heap in MB.
* `skywalking_zoo_port`: ZooKeeper server listens.
* `skywalking_zoo_enable_auth`: Whether enable quorum authentication using SASL.
* `skywalking_zoo_user_super_passwd`: Administrator priviledges user password.
* `skywalking_zoo_user_client_arg`: Client authentication information.

##### System Variables
* `skywalking_arg.ulimit_core`: The number of core dump launched by systemd.
* `skywalking_arg.ulimit_nofile`: The number of files launched by systemd.
* `skywalking_arg.ulimit_nproc`: The number of processes launched by systemd.
* `skywalking_arg.user`: SkyWalking Service running user.

##### Service Mesh
* `environments`: Define the service environment.
* `tags`: Define the service custom label.
* `exporter_is_install`: Whether to install prometheus exporter.
* `consul_public_register`: false Whether register a exporter service with public consul client.
* `consul_public_exporter_token`: Public Consul client ACL token.
* `consul_public_http_prot`: The consul Hypertext Transfer Protocol.
* `consul_public_clients`: List of public consul clients.
* `consul_public_http_port`: The consul HTTP API port.

### Other parameters
There are some variables in vars/main.yml:

## Dependencies
- Ansible versions >= 2.8 are supported.
- Python >= 2.7.5 
- [Elasticsearch](https://github.com/goldstrike77/ansible-role-linux-elasticsearch.git)
- [ZooKeeper](https://github.com/goldstrike77/ansible-role-linux-zookeeper.git) 

## Example

### Hosts inventory file
See tests/inventory for an example.

### Vars in role configuration
Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: all
      roles:
         - role: ansible-role-linux-skywalking

### Combination of group vars and playbook
You can also use the group_vars or the host_vars files for setting the variables needed for this role. File you should change: group_vars/all or host_vars/`group_name`

    skywalking_cluster: 'demo'
    skywalking_path: '/data'
    skywalking_version: '7.0.0'
    skywalking_oap_jvm_xmx: '1024'
    skywalking_web_jvm_xmx: '512'
    skywalking_datattl:
      record: '5'
      otherMetrics: '30'
      monthMetrics: '6'
    skywalking_elastic_dept: true
    skywalking_zoo_dept: true
    skywalking_port_arg:
      grpc: '11800'
      rest: '12800'
      ui: '18080'
      exporter: '1234'
    skywalking_elastic_auth: true
    skywalking_elastic_cluster: '{{ skywalking_cluster }}'
    skywalking_elastic_hosts:
      - 'localhost'
    skywalking_elastic_pass: 'changeme'
    skywalking_elastic_path: '{{ skywalking_path }}'
    skywalking_elastic_port_rest: '9200'
    skywalking_elastic_user: 'elastic'
    skywalking_elastic_version: '7.5.2'
    skywalking_elastic_heap_size: '2g'
    skywalking_zoo_version: '3.5.8'
    skywalking_zoo_cluster: '{{ skywalking_cluster }}'
    skywalking_zoo_path: '{{ skywalking_path }}'
    skywalking_zoo_servers:
      - 'localhost'
    skywalking_zoo_jvm_xmx: '2048'
    skywalking_zoo_port:
      admin: '18080'
      client: '2181'
      jmx: '9405'
      leader: '2888'
      election: '3888'
    skywalking_zoo_enable_auth: false
    skywalking_zoo_user_super_passwd: 'changeme'
    skywalking_zoo_user_client_arg:
      - user: 'skywalking'
        passwd: 'changeme'
    skywalking_arg:
      ulimit_core: 'infinity'
      ulimit_nofile: '10240'
      ulimit_nproc: '10240'
      user: 'skywalking'
    environments: 'Development'
    tags:
      subscription: 'default'
      owner: 'nobody'
      department: 'Infrastructure'
      organization: 'The Company'
      region: 'IDC01'
    exporter_is_install: false
    consul_public_register: false
    consul_public_exporter_token: '00000000-0000-0000-0000-000000000000'
    consul_public_http_prot: 'https'
    consul_public_http_port: '8500'
    consul_public_clients:
      - '127.0.0.1'

## License
![](https://img.shields.io/badge/MIT-purple.svg?style=for-the-badge)

## Author Information
Please send your suggestions to make this role better.

## Contributors
Special thanks to the [Connext Information Technology](http://www.connext.com.cn) for their contributions to this role.
