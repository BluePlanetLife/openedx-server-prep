# Open edX Server Preparation

## Introduction
The goal of the BluePlanetLife/edx-server-prep project is to provide a simple, but flexible way for anyone to prepare a server(s) for the installation of an instance of [Open edX](http://openedx.org), especially when not using AWS or Vagrant..

The preparation is managed by [Ansible](http://ansible.com/) and is intended for the use with [edx/configuration](https://github.com/edx/configuration).

## Coverage

### Installation types covered
Currently the playbook is created for the purpose of preparing an [edx Ubuntu 12.04 64-bit single server installation](https://github.com/edx/configuration/wiki/edX-Ubuntu-12.04-64-bit-Installation) as described on the [edx/edx-platform](https://github.com/edx/edx-platform) wiki pages.

Now also supporting Backup and Restore playbooks. Note: For now this playbook only dumps and restores the MySQL containing Users and MongoDB containing Courses.

### Functions and Options
* Prepare Configuration Files
  * Transfer the configuration from a local folder
  * Clone configuration from a git repository
* Select an Open edX version
  * release
  * master
  * named-release/birch ([Named-Releases](https://github.com/edx/configuration/wiki/Named-Releases))
* System Settings
  * Time Zone
  * Locale
* Additional Packages
  * htop
* Backup and Restore
  * Backup to the the machine executing the playbook (can be set to localhost)
  * Restore the backup just created
  * Your previous backups will remain in the files/backups folder

## Requirements
* [Ansible](http://docs.ansible.com/intro_installation.html) installed locally
* Remote (or VM) Ubuntu 12.04 64-bit Server to prepare
* [Further requirements for Open edX](https://github.com/edx/configuration/wiki/edX-Ubuntu-12.04-64-bit-Installation#hardware-requirements)

## Installation & Customization
1. Prepare your workstation
  - install virtualenv
  - activate that virtualenv
  - `pip install ansible` within that virtualenv
2. Clone 
3. Add an IP address to the `hosts` file, underneath `[all]`. 
  - Use ip address of vagrant box if using vagrant
4. Edit the file you want to use
  - Edit `user: ubuntu` to match your user for SSH. Use `vagrant` if using vagrant.
  - Define the hosts you want to have prepared (default: `all`)
5. Edit `Playbook` and `System` Variables in the 1. playbook (`var: default`)
  - Use a local configuration folder (`local_conf: False`)
  - Name of the local configuration folder (`config_folder: configuration`)
  - Repository for remote configuration (`config_repo: git@github.com:edx/configuration.git`)
  - Open edX version to clone from GitHub (`openedx_version: named-release/birch`). You may want `named-release/aspen`
  - Select your time zone (`timezone: Europe/Zurich`)
  - Set your locale settings (`locale: en_GB.UTF-8`)
  
## Running the playbook
(More information on [Ansible Playbooks](http://docs.ansible.com/playbooks.html))

Open your command line interface and navigate to the cloned repository.
After customizing your hosts and 1-ubuntu1204_single_server.yml file, run the following command:
```bash
ansible-playbook -i hosts 1-ubuntu1204_single_server.yml
```
If using vagrant, make sure `vagrant` user has permissions to write to `/home/vagrant/.ansible` folder.
```bash
sudo chown -R vagrant:vagrant /home/vagrant/.ansible
```
### Backup and Restore
After customizing your hosts and backup/restore files, run the following command for a backup:
```bash
ansible-playbook -i hosts 98-db-backup.yml
```
or for Restore that backup
```bash
ansible-playbook -i hosts 99-db-restore.yml
```



! Custom Configuration !
In case you have a customized configuration, make sure to place it one the same folder level as this folder:
```
your_repos_folder   
|--configuration-custom
|--preparation
|--...
```

Finally you can run the edx_sandbox.yml playbook, according to the wiki pages of [edx/configuration](https://github.com/edx/configuration/wiki/edX-Ubuntu-12.04-64-bit-Installation):
`cd /var/tmp/configuration/playbooks && sudo ansible-playbook -c local ./edx_sandbox.yml -i "localhost,"`

## Future Plans
This is a first script for simple and basic use and preparation. Further playbooks are planned for preparing different scenarios and constellations of Open edX instances.

*Next up:* Split Server Preparation
For setting up two servers: 
- App server
- DB server

## Format
Scripts follow the official [Ansible Coding Conventions](https://github.com/edx/configuration/wiki/Ansible-Coding-Conventions) as well as the [Ansible variable convention and overriding defaults](https://github.com/edx/configuration/wiki/Ansible-variable-conventions-and-overriding-defaults), defined by and used for Open edX.
