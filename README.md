# ansible-prometheus-installer
Deploy and configure Prometheus monitoring system 

<p><img src="https://cdn.worldvectorlogo.com/logos/prometheus.svg" alt="prometheus logo" title="prometheus" align="right" height="60" /></p>

# Ansible Role: prometheus

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




#### Short version

`prometheus_targets` is just a map used to create multiple files located in "{{ prometheus_config_dir }}/file_sd" directory. Where file names are composed from top-level keys in that map with `.yml` suffix. Those files store [file_sd scrape targets data](https://prometheus.io/docs/prometheus/latest/configuration/configuration/#file_sd_config) and they need to be read in `prometheus_scrape_configs`.

#### Example

Lets look at our default configuration, which shows all features. By default we have this `prometheus_targets`:
```
prometheus_targets:
  node:  # This is a base file name. File is located in "{{ prometheus_config_dir }}/file_sd/<<BASENAME>>.yml"
    - targets:              #
        - localhost:9100    # All this is a targets section in file_sd format
      labels:               #
        env: test           #
```
Such config will result in creating one file named `node.yml` in `{{ prometheus_config_dir }}/file_sd` directory.

Next this file needs to be loaded into scrape config. Here is modified version of our default `prometheus_scrape_configs`:
```
prometheus_scrape_configs:
  - job_name: "prometheus"    # Custom scrape job, here using `static_config`
    metrics_path: "/metrics"
    static_configs:
      - targets:
          - "localhost:9090"
  - job_name: "example-node-file-servicediscovery"
    file_sd_configs:
      - files:
          - "{{ prometheus_config_dir }}/file_sd/node.yml" # This line loads file created from `prometheus_targets`
```

## Example



## Local Testing

1. Download the prometheus binary for Linux at role/prometheus/file.
2. Edit the grop_vars/all to change variable values 
        Current Variable: 
        prom_ver: 2.12.0
        node_exp: 0.18.1
        ins_dir: /opt
        
 3. Run all roles
    ansible-playbook -i hosts monitoringServer.yml [It will install ntp,prometheus and NodeExported]
 4. Run only prometheus or NodeExporter
    ansible-playbook -i hosts monitoringServer.yml --tags=prometheus
    ansible-playbook -i hosts monitoringServer.yml --tags=prometheus-nodeExporter
 5. Example of hosts file
    [prometheus]
    172.17.254.80

    [node_exporter]
    172.17.254.81
    172.17.254.82
