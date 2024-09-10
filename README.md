# Luganodes SRE Task

## Task 4: Grafana Alloy Deployment via Ansible with Monitoring

### Objective
Create an Ansible playbook to deploy Grafana Alloy on a host running a self-hosted Cosmos chain (using the Ignite CLI). Set up Grafana and Mimir for collecting and visualizing metrics from the Grafana Alloy instance.

### Features
1. **Ansible Playbook**: Develop an Ansible playbook that automates the deployment and configuration of Grafana Alloy on a server hosting the Cosmos chain.

2. **Grafana & Mimir Setup**: Install and configure both Grafana and Mimir for monitoring, ensuring that metrics from the Grafana Alloy instance are collected and visualized.

3. **Bonus - Alerting**: (Optional) Implement alerting via Telegram and demonstrate how the alerting system functions.

### Deliverables

1. **Documentation**: Provide detailed documentation that explains each step of the playbook creation and the deployment of Mimir and Grafana.

2. **Ansible Playbook**: Upload the completed playbook to a public GitHub repository (with a link).

3. **Configuration Files**: Provide the Grafana and Mimir configuration files in the same GitHub repository (with a link).

4. **Video Demonstration**:
Show the Grafana dashboard displaying node exporter metrics. Demonstrate the Prometheus interface (showing all configured targets).
If the bonus task is completed, showcase the alerts within Grafana. Provide a video link for the demonstration.

## EC2 Provisioning and Keyless SSH Configuration

### EC2 Instances

For this project, three EC2 `t2.micro` instances have been provisioned on AWS. The instances are configured as follows:

1. **Master Instance**: The primary node that manages and controls the worker nodes.
2. **Worker Instance 1**: A worker node that performs tasks assigned by the master.
3. **Worker Instance 2**: Another worker node for distributing the load and tasks.

### Provisioning Steps

1. **Provision EC2 Instances**:
   - Three `t2.micro` EC2 instances are created using the AWS Management Console.
   - Ensure that all instances are in the same VPC and have appropriate security group settings to allow SSH access.

2. **Configure Keyless SSH Access**:

   **On the Master Instance**:
   1. Generate an SSH key pair (if not already generated):
      ```bash
      ssh-keygen
      ```
   2. Copy the public key:
      ```bash
      cat ~/.ssh/id_rsa.pub
      ```

   **On Each Worker Instance**:
   1. Generate an SSH key pair:
      ```bash
      ssh-keygen
      ```
   2. Add the public key of the master instance to the `authorized_keys` file:
      ```bash
      echo "PUBLIC_KEY_OF_MASTER_INSTANCE" >> ~/.ssh/authorized_keys
      ```
      Replace `PUBLIC_KEY_OF_MASTER_INSTANCE` with the actual public key copied from the master instance.

3. **Verify Keyless SSH Access**:
   - From the master instance, test SSH access to each worker instance without being prompted for a password:
     ```sh
     ssh ec2-user@WORKER_INSTANCE_1_IP
     ssh ec2-user@WORKER_INSTANCE_2_IP
     ```

## Project Structure
├──  assign-task
│  ├──  file.json
│  ├──  proof.png
│  └──  script.py
├──  inventory
│  └──  hosts.ini
├──  main.yml
├──  README.md
├──  roles
│  ├──  alloy
│  │  ├──  files
│  │  │  └──  config.alloy
│  │  ├──  handlers
│  │  │  └──  main.yml
│  │  ├──  meta
│  │  │  └──  main.yml
│  │  ├──  README.md
│  │  └──  tasks
│  │     └──  main.yml
│  ├──  dependencies
│  │  ├──  meta
│  │  │  └──  main.yml
│  │  ├──  README.md
│  │  └──  tasks
│  │     └──  main.yml
│  ├──  grafana
│  │  ├──  files
│  │  ├──  handlers
│  │  │  └──  main.yml
│  │  ├──  meta
│  │  │  └──  main.yml
│  │  ├──  README.md
│  │  └──  tasks
│  │     └──  main.yml
│  ├──  ignite
│  │  ├──  handlers
│  │  │  └──  main.yml
│  │  ├──  meta
│  │  │  └──  main.yml
│  │  ├──  README.md
│  │  └──  tasks
│  │     └──  main.yml
│  ├──  mimir
│  │  ├──  files
│  │  │  └──  mimir.yml
│  │  ├──  handlers
│  │  │  └──  main.yml
│  │  ├──  meta
│  │  │  └──  main.yml
│  │  ├──  README.md
│  │  └──  tasks
│  │     └──  main.yml
│  └──  prometheus
│     ├──  files
│     │  └──  prometheus.yml
│     ├──  handlers
│     │  └──  main.yml
│     ├──  meta
│     │  └──  main.yml
│     ├──  README.md
│     └──  tasks
│        └──  main.yml
└──  script.sh

## Ansible Roles

Instead of writing the entire playbook into a single `main.yml`, the configuration management is split into multiple Ansible roles for better organization and modularity. These roles are created using ansible-galaxy. Below is a description of each role:

```bash
ansible-galaxy role init <ROLE_NAME>
```

### Writeup about the Ansible Roles

- **`alloy`**: Manages the deployment and configuration of the Grafana Alloy container, including setting up configuration files and running the Docker container with the necessary volume mappings.
  
- **`dependencies`**: Handles the installation of essential dependencies required for the project, including setting up necessary software and tools.

- **`grafana`**: Configures and deploys Grafana, including setting up the Grafana container, waiting for it to be fully operational, and configuring Prometheus as a data source.

- **`ignite`**: Manages the deployment of the Ignite CLI and configuration of the self-hosted Cosmos chain.

- **`mimir`**: Oversees the deployment and configuration of the Mimir container, including managing the necessary configuration files and running the Docker container with the appropriate settings.

- **`prometheus`**: Takes care of deploying and configuring the Prometheus container, ensuring that it is set up with the correct configuration files and running properly.
