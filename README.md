# BINGE ANSIBLE

This repository contains Ansible playbooks to set up prerequisite services for BINGE applications on servers. It's designed to be easily cloned and adapted for your own server infrastructure.

## Repository Structure

This repository is organized to support multiple server types, each with specific roles:

1. **Application Servers (binge_servers group)**
   - Receive Apache and Node.js installations
   - Used for running BINGE applications
   - Managed by the `binge-services-playbook.yml`

2. **Jenkins Server (jenkins_servers group)**
   - Receives only Jenkins installation
   - Used for CI/CD pipelines
   - Managed by the `jenkins-playbook.yml`

## Prerequisite Services

### Application Servers
- **Apache** - Web server
- **Node.js 20** - JavaScript runtime
- **Nodemon** - Utility for auto-restarting Node applications

### Jenkins Server
- **Jenkins** - Continuous Integration/Continuous Deployment server
- **Java JDK 11** - Required for Jenkins

## Getting Started

### 1. Clone this Repository

```bash
git clone https://github.com/yourusername/BINGE-ANSIBLE.git
cd BINGE-ANSIBLE
```

### 2. Configure Your Server Inventory

Edit the inventory file at `infra/ansible/config/binge/inventory.ini`:

```ini
# Application Servers - Add your servers here
[binge_servers]
app-server-1 ansible_host=YOUR_APP_SERVER_IP ansible_user=root ansible_ssh_private_key_file=private_key.pem
app-server-2 ansible_host=YOUR_APP_SERVER_IP ansible_user=root ansible_ssh_private_key_file=private_key.pem

# Jenkins Server - Add your Jenkins server here
[jenkins_servers]
jenkins-server ansible_host=YOUR_JENKINS_SERVER_IP ansible_user=root ansible_ssh_private_key_file=private_key.pem

# Common SSH settings for all servers
[all:vars]
ansible_ssh_common_args='-o StrictHostKeyChecking=no'
```

Replace:
- `YOUR_APP_SERVER_IP` with your application server IP addresses
- `YOUR_JENKINS_SERVER_IP` with your Jenkins server IP address
- Adjust the `ansible_user` if you're not using root

### 3. Configure Variables (Optional)

You can customize the installation by editing `infra/ansible/config/binge/vars.yml`:

```yaml
# Node.js configuration
node_version: "20"  # Change if you need a different Node.js version

# Apache configuration
apache_packages:
  - apache2
  - apache2-utils

# Jenkins configuration
jenkins_port: 8080  # Change if you need a different port
jenkins_java_version: "openjdk-11-jdk"
```

## Running the Playbooks

### Option 1: Running Via GitHub Actions

If you're using GitHub, you can set up the workflow:

1. Set up the SSH key as a GitHub secret:
   - Go to your GitHub repository
   - Click on "Settings" tab
   - Select "Secrets and variables" → "Actions" from the left sidebar
   - Click on "New repository secret"
   - Name it `GCP_VM_PRIVATE_KEY` and paste your private key content
   - Click "Add secret"

2. Run the workflow:
   - Go to the "Actions" tab
   - Select "BINGE Services Installation" workflow
   - Click "Run workflow"
   - Choose which playbook to run:
     - `binge-services` - For application servers
     - `jenkins` - For Jenkins server
   - Click "Run workflow"

### Option 2: Running Manually via CLI

If you prefer to run Ansible directly:

```bash
# First, ensure you have Ansible installed
# Ubuntu/Debian: sudo apt install ansible
# CentOS/RHEL: sudo yum install ansible
# macOS: brew install ansible

# To install services on application servers
ansible-playbook infra/ansible/binge-services-playbook.yml -i infra/ansible/config/binge/inventory.ini -e "env=binge"

# To install Jenkins on the Jenkins server
ansible-playbook infra/ansible/jenkins-playbook.yml -i infra/ansible/config/binge/inventory.ini -e "env=binge"
```

## What the Playbooks Do

### BINGE Services Playbook (binge-services-playbook.yml)

This playbook runs ONLY on servers in the `binge_servers` group and:
1. Checks current status of all services
2. Installs Apache web server
3. Installs Node.js 20
4. Installs Nodemon globally

### Jenkins Playbook (jenkins-playbook.yml)

This playbook runs ONLY on servers in the `jenkins_servers` group and:
1. Installs Java JDK 11
2. Adds Jenkins repository and GPG key
3. Installs Jenkins
4. Ensures Jenkins is running and configured to start at boot
5. Displays the initial admin password for first-time setup
6. Shows the URL to access the Jenkins dashboard

## Customizing for Your Project

1. **Server Groups**: You can add more server groups in the inventory file as needed
2. **Additional Roles**: Create new roles in `infra/ansible/roles/` for other services
3. **Custom Playbooks**: Create new playbooks targeting specific server groups

## Troubleshooting

If you encounter any issues during the installation:

1. **Connection Issues**:
   - Check that your servers are accessible via SSH with the provided key
   - Verify firewall rules allow SSH connections
   - Ensure the SSH key has proper permissions (chmod 600 private_key.pem)

2. **Permission Issues**:
   - Verify the user specified in inventory has sudo/root access
   - Check file permissions on the target servers

3. **Service Issues**:
   - Check service status: `systemctl status apache2` or `systemctl status jenkins`
   - Review logs: `/var/log/apache2/error.log` or `/var/log/jenkins/jenkins.log`

4. **GitHub Actions Issues**:
   - Verify secrets are properly configured
   - Check the workflow logs for detailed error messages

## Contributing

If you enhance this repository with additional roles or improvements, please feel free to submit a pull request.

## License

This project is licensed under the MIT License - see the LICENSE file for details.
