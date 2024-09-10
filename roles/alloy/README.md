Grafana Alloy
=========

This ansible role deploys grafana alloy on the hosts inside a docker container, 
configuring it with the provided configuration file in the files directory.

The config file is attached as a docker volume to /etc/alloy/config.alloy

Requirements
------------

- docker and docker-compose should be installed on the target server.
- The `docker_container` Ansible module requires Docker to be running and accessible on the target server.


