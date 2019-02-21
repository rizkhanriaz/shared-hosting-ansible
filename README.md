LAMP Stack for Shared Hosting
================

Setup your LAMP server quickly with Ansible. This repository is dedicated to `CentOS 7` system.
Main goal is to setup working LAMP server with multiple accounts and to easily manage. 
Secondary goal is to keep your server secure and up to date. 

How to use?
-----------

Make sure that you have Ansible installed. Minimal version required for the included roles and playbooks is `2.0`.

First create your Inventory file. You can work entirely in this repository. Name your Inventory file `hosts` (this file is in .gitignore) and setup all required connections there.

Example Inventory file:
```
[web-servers]
206.189.82.103	ansible_ssh_user=centos   ansible_ssh_private_key_file="~/.ssh/id_rsa"
```

Next setup your playbook. The easiest way is to copy `playbooks/example-*.yml` files without the `example` prefix. 

And copy all variable files inside `vars` directory without the `example` prefix to override defaults in roles. All `.yml` files in `vars` directory except for the files with `example` prefix are in .gitignore


Run the following command to start the server provisioning:
```
ansible-playbook -i hosts playbooks/provision.yml
```

Some tasks are marked with `healthcheck` tag. They will do some basic checks to see if system is up and running. All tasks should be green. If there are tasks marked as changed, something is not OK.
```
ansible-playbook -i hosts playbooks/provision.yml --tags="healthcheck" 
``` 

Public key is required for logging in via SSH with RSA keys. Logging with password will be turned off.
Password is required for sudo, if you will set `centos_groups_wheel_password_required` to `yes` (this is default value). Once sudo with password will be available, you must execute playbooks with `-K` argument and pass sudo password:

```
ansible-playbook -i hosts playbooks/YOUR_PLAYBOOK_FILE_NAME.yml -K
```

Included roles
--------------

**centos** - Takes care of system settings. Configure yum, yum-cron, update and remove packages. 
It also take cares of groups and users. Enable sudo. Configure secure SSH. Set up firewall based on iptables.

**ntp** - Takes care of system timezone and NTP server. It uses Chrony for using NTP.

**openssl** - Compile OpenSSL from source

**git** - Compile git from source with OpenSSL

**httpd** - Compile and configure Apache httpd from source with OpenSSL

**php** - Install and configure Multiple PHP versions, PHP-FPM and Composer. with Opcache.

**vhost** - Manage multiple virtual hosts with dedicated user and different php versions.


## Authors

* **Rizkhan Riaz** - 
[GitHub](https://github.com/rizkhanriaz)


## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details
