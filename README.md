# Ansible Playbook for Ecommerce Application Deployment

This Ansible playbook is designed to automate the deployment of an ecommerce application stack, including Docker setup, application installation, database configuration, and the installation and configuration of proxy, InfluxDB, and Grafana components.

## Prerequisites

Ensure that you have Ansible installed on your control machine, and the target nodes are reachable from the control machine. Additionally, make sure you have the necessary permissions to perform the tasks, as the playbook uses `become: yes` for privilege escalation.

## Playbook Structure

The playbook is organized into distinct roles:

1. **Prepare Docker:**
   - Installs Docker on designated nodes (`app_nodes` and `lb_nodes`).
   - Starts the Docker service.

2. **Install Ecommerce Application:**
   - Sets up the ecommerce application on `app_nodes`.
   - Copies Dockerfile template and builds a Docker image.
   - Runs a Docker container for the application.

3. **Setup Database:**
   - Installs required software and Python modules on `db_nodes`.
   - Configures MariaDB service, creates a database, and sets up a user with remote access.

4. **Install and Configure Proxy, InfluxDB, and Grafana:**
   - On `lb_nodes`:
     - Installs Nginx as a proxy.
     - Configures InfluxDB and Grafana in Docker containers.
     - Restarts Nginx and Grafana.

5. **Install and Configure Telegraf on All Nodes:**
   - Installs Telegraf on all nodes.
   - Copies Telegraf configuration file.
   - Starts the Telegraf service.

## How to Use

1. Customize Variables: Edit the variables in the playbook according to your environment. Variables are typically defined in a separate file, but you can also customize them directly in the playbook.

2. Run the Playbook: Execute the playbook using the following command:

    ```bash
    ansible-playbook setup.yaml -i hosts.ini --extra-vars "@vars.yaml"
    ```

   Replace `inventory.yml` with your inventory file.

3. Monitor the Deployment: Access the Grafana dashboard at http://<lb_node_ip>:8080 to monitor the application and system metrics.
