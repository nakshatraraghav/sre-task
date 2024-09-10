Prometheus
=========

This Ansible role starts prometheus inside a docker container on the hosts mentioned
in the inventory file, and configures it using the provided config file, in the files
directory, Grafana Alloy should be configured to send metrics to this Prometheus Container

Requirements
------------

- Docker and Docker Compose should be installed on the target server.
- The `docker_container` Ansible module requires Docker to be running and accessible on the target server.

Dependencies
------------

Please make sure that the prometheus.yml is there in the files directory, because
it would be attached as a docker volume onto the container running prometheus

Grafana Alloy Configuration
---------------------------
Ensure that Grafana Alloy is configured to send metrics to this Prometheus instance. You can configure Grafana Alloy by updating its configuration file to include Prometheus as a data source. For example: