
<p><img src="https://cdn.worldvectorlogo.com/logos/prometheus.svg" alt="prometheus logo" title="prometheus" align="right" height="60" /></p>

# Ansible-prometheus-installer
`Deploy and configure Prometheus monitoring system`

[![Build Status](https://travis-ci.org/cloudalchemy/ansible-prometheus.svg?branch=master)](https://travis-ci.org/cloudalchemy/ansible-prometheus)


## Description

Deploy [Prometheus](https://github.com/prometheus/prometheus) monitoring system using ansible.



## Requirements

- Ansible >= 2.8 (Tested on MacOS ver: ansible 2.8.4)


## Role Variables

All variables which can be overridden are stored in [defaults/main.yml](defaults/main.yml) file as well as in table below.

| Name           | Default Value | Description                        |
| -------------- | ------------- | -----------------------------------|
| `prometheus_version` | 2.12.0 | Prometheus package version. Also accepts `latest` as parameter. Only prometheus 2.x is supported |
| `prometheus_config_dir` | /etc/prometheus | Path to directory with prometheus configuration |
| `prometheus_db_dir` | /var/lib/prometheus | Path to directory with prometheus database |
| `prometheus_web_listen_address` | "0.0.0.0:9090" | Address on which prometheus will be listening |



## How To Run

1. Download the prometheus binary for Linux at role/prometheus/file.
2. Edit the grop_vars/all to change variable values 

```
        Current Variable: 
        prom_ver: 2.12.0
        node_exp: 0.18.1
        ins_dir: /opt
 ```    
        
 3. Run all roles
 
 ```
    ansible-playbook -i hosts monitoringServer.yml [It will install ntp,prometheus and NodeExported]
 ```   
 4. Run only prometheus or NodeExporter
 
 ```
    ansible-playbook -i hosts monitoringServer.yml --tags=prometheus
    ansible-playbook -i hosts monitoringServer.yml --tags=prometheus-nodeExporter
 ```   
 
 5. Update the host file 
 
