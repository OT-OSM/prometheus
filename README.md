# Ansible Role: Prometheus

Prometheus is an opensource monitoring solution that gathers time series based numerical data. It is a project which was started by Google’s ex-employees at SoundCloud.

To monitor your services and infra with prometheus your service need to expose an endpoint in the form of port or url. For ex:- {{ localhost:9090 }}. The endpoint is HTTP interface that exposes the metrics.

### This role is compatible with [Linux Hardened](https://github.com/OT-OSM/linux_armour) OS

Version History
---------------

|**Date**| **Version**| **Description**| **Changed By** |
|----------|---------|---------------|-----------------|
|**Feb  2019** | v0.0.1 | Initial Draft | [Abhishek Dubey](abhishek.dubey@opstree.com) |
|**July 2020** | v0.1.0 | Added Service Discovery Feature| [Mahesh Kumar](mahesh.kumar@opstree.com) |

## Requirements

There is no particular requirment for running this role. As this role is platform independent for centos 6 or 7 and ubuntu 14 or 16. The only dependency for centos 6 is libselinux python and we have included that as well.
The basic requirments are:-
- Centos/Ubuntu Server
- Python should be installed on the target server so that ansible can perform task
- libselinux-python should be available on Centos 6 so that ansible can connect to it

## Role Variables

```yaml
# defaults file for prometheus
prometheus_version: "2.3.2"
prometheus_ip: "0.0.0.0"
prometheus_port: "9090"
base_download_url: "https://github.com/prometheus/prometheus/releases/download"
user_name: "prometheus"
group_name: "prometheus"
prometheus_binary_path: "/usr/local/bin"

# please enter telegraf or node_exporter
agent_discovery: "no"
agent: "telegraf"
tgraf_port: "9273"
node_exporter_port: "9100"
env: prod
```

You can define any prometheus version that you want to install on your server.

#### Mandatory Variables

|**Variable**|**Default Value**|**Possible Values**|**Description**|
|------------|-----------------|-------------------|---------------|
|prometheus_version | "2.3.2" | Any Version | Prometheus will be downloaded from github releases, so you have to define version |
|prometheus_ip | "0.0.0.0" | | IP of the server on which prometheus will expose its UI |

#### Optional Variables

|**Variable**|**Default Value**|**Possible Values**|**Description**|
|------------|-----------------|-------------------|---------------|
|prometheus_port | "9090" | Prometheus Service Port | Port no. of server on which prometheus should listen |
|base_download_url | "https://github.com/prometheus/prometheus/releases/download" | Base url  of Prometheus's Download link | Base url of prometheus release |
|user_name | prometheus | Any User | Prometheus User Name |
|group_name | prometheus | Any Group | Promtheus Group Name |
|prometheus_binary_path| "/usr/loca/bin" | Any Path | Path to Copy Prometheus Binary |
|agent_discovery | "no" | "yes"/"no" | Enable of Disable Agent Discovery, Agent Discovery added all the agents in Promtheus Configuration |
|agent | "telegraf" | "telegraf"/"node_exporter" | Which agent you are using to get metrics. Supported Values are "node_exporter" and "telegraf" |
|env | "prod" | Any Enviornment Name | Enviornment, in which you agents are running |
|tgraf_port | "9273" | telegraf service port | if you are using telegraf, Port no. of telegraf on which telegraf is listening |
|node_exporter_port | "9100" | Node Exporter Service Port | if Node Exporter is using as agent, Port no. of telegraf on which telegraf is listening |

## Dependencies
- If Service Discovery is Enable, install one of telgraf and node exporter agent on each node, which must be added in prometheus targets.

## Directory Structure
Directory structure:-
```bash
osm_prometheus
├── defaults
│   └── main.yml
├── handlers
│   └── main.yml
├── hosts
├── README.md
├── site.yml
├── tasks
│   ├── debian.yml
│   ├── main.yml
│   └── redhat.yml
└── templates
    ├── prometheus.init.j2
    ├── prometheus.service.j2
    └── prometheus.yml.j2
```
## Example Playbook if Agent Discovery is not Enable

Here is an example for the main playbook

```yaml
---
- hosts: prometheus
  user: test-user
  become: true
  roles:
    -  prometheus
```

For inventory you can create a host file in which you can define your server ip, For example:-
```
[prometheus]
10.1.1.100
```

## Example Playbook if Agent Discovery is Enable
Here is an example for the main playbook. Following Playbook Gather Facts of each Node, where agents is installed. Because of Gather Facts, Ansible Controller storing ip address of each host and adding it in promtheus configuration.

```yaml
---
- name: Gather Facts of Agent Nodes
  hosts: agents
  gather_facts: true
  tasks: []

- hosts: prometheus
  user: test-user
  become: true
  roles:
    -  prometheus
```

For inventory you can create a host file in which you can define your server ip, For example:-
```
[prometheus]
10.1.1.100

[agents]
10.1.1.100
10.1.1.101
10.1.1.102
```

You can simply use this role by using this command
```shell
ansible-playbook -i hosts site.yml
```
## License

GNU

## Author Information

This role is written and maintained by [Abhishek Dubey](https://github.com/iamabhishekdubey). If you have any queries and sugesstion, please feel free to reach.
