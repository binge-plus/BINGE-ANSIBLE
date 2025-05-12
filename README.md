# BINGE ANSIBLE

This repository contains Ansible playbooks to set up prerequisite services for BINGE applications on servers.

## Prerequisite Services Installed

- **Apache** - Web server
- **Node.js 20** - JavaScript runtime
- **Nodemon** - Utility for auto-restarting Node applications
- **NVM** - Node Version Manager for managing multiple Node.js versions
- **PM2** - Process Manager for Node.js applications with auto-restart capability

## Server Configuration

The playbook is configured to run on the following servers:
- **binge-plus**: 35.192.69.127
- **binge-admin**: 34.136.40.60

## GitHub Secrets Setup

You need to set up the following GitHub secrets:

1. **SSH_KEY_FILE**: Your private SSH key that has access to both servers
   - This should be the complete private key content, including the `-----BEGIN OPENSSH PRIVATE KEY-----` and `-----END OPENSSH PRIVATE KEY-----` lines
   - The key must have root access to the specified servers

To add these secrets:
1. Go to your GitHub repository
2. Click on "Settings" tab
3. Select "Secrets and variables" → "Actions" from the left sidebar
4. Click on "New repository secret"
5. Add the SSH_KEY_FILE with your private key content
6. Click "Add secret"

## Running the Ansible Playbook

The playbook will only run on the `main` branch. To run it:

1. Go to the GitHub repository
2. Navigate to "Actions" tab
3. Select "BINGE Services Installation" workflow
4. Click "Run workflow"
5. Select the "main" branch from the dropdown
6. Click "Run workflow"

## What the Playbook Does

1. **Checks Current Status**:
   - Checks if Apache is already installed and running
   - Checks if Node.js 20 is already installed
   - Checks if nodemon is already installed
   - Checks if NVM is already installed
   - Checks if PM2 is already installed

2. **Installs Missing Services**:
   - Only installs services that are not present or running
   - Ensures all services are properly configured

3. **Configures Advanced Features**:
   - Sets up NVM with the latest LTS Node.js version
   - Configures PM2 to start on system boot

4. **Reports Status**:
   - Provides feedback on which services were already installed
   - Reports on newly installed services

## Troubleshooting

If you encounter any issues during the installation:

1. Check that your servers are accessible via SSH with the provided key
2. Verify that the SSH key has root permissions on the servers
3. Ensure GitHub Actions has the correct secrets configured
4. Check the GitHub Actions logs for detailed error messages
