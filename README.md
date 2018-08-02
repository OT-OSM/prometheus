# Ansible Role for Prometheus

Prometheus is an opensource monitoring solution that gathers time series based numerical data. It is a project which was started by Google’s ex-employees at SoundCloud.
To monitor your services and infra with prometheus your service need to expose an endpoint in the form of port or url. For ex:- {{ localhost:9090 }}. The endpoint is HTTP interface that exposes the metrics.

## Requirements

There is no particular requirment for running this role. As this role is platform independent for centos 6 or 7 and ubuntu 14 or 16. The only dependency for centos 6 is libselinux python and we have included that as well.
The basic requirments are:-
- Centos/Ubuntu Server
- Python should be installed on the target server so that ansible can perform task
- 9090 port should be open in your server where you can see prometheus dashboard
- Libselinux-python should be available on Centos 6 so that ansible can connect to it

## Role Variables
The role variables are defined in the [vars](https://gitlab.com/oosm/osm_prometheus/tree/master/vars). So there is not so many variables you just have to pass the prometheus version.

```yaml
# vars file for prometheus
prometheus_version: "2.3.2"
```
You can define any prometheus version that you want to install on your server

## Dependencies

There is no further dependency for this role you just need a server with 9090 port open. Other than this for Centos 6 we are using init script as systemd module is not available.


## Directory Structure
Directory structure:-
```
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
## Example Playbook

Here is an example for the main playbook

```yaml
---
- hosts: prometheus
  user: root
  roles:
    -  prometheus
```
Here We are using root as an user but you can use different user, For that you just have to make become value true. Something like this:-
```yaml
---
- hosts: prometheus
  user: test-user
  become: true
  roles:
    -  prometheus
```
You can simply use this role by using this command
```shell
ansible-playbook -i hosts site.yml
```
## License

BSD

## Author Information

This role is written and maintained by [Abhishek Dubey](https://gitlab.com/abhishek-dubey). If you have any queries and sugesstion, please feel free to reach.