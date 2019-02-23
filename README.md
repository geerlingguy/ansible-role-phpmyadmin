# Ansible Role: phpMyAdmin

[![Build Status](https://travis-ci.org/geerlingguy/ansible-role-phpmyadmin.svg?branch=master)](https://travis-ci.org/geerlingguy/ansible-role-phpmyadmin)

Installs phpMyAdmin on RHEL/CentOS/Debian/Ubuntu.

phpMyAdmin is a simple interface for interacting with MySQL databases via a web browser. It is not necessarily the most secure or efficient method of managing databases, but for those who need a GUI, this one is better than many others. I (geerlingguy) would personally never run it on a production server, nor do I use it myself (I use Sequel Pro or simply interact with the database via CLI/APIs), but it seems many people like it (especially people stuck on a Windows machine with no good MySQL GUIs :).

## Requirements

**RedHat/CentOS**: Requires the EPEL repository on RedHat/CentOS 6.x hosts. You can install the EPEL repository using the `geerlingguy.repo-epel` role.

**Debian/Ubuntu**: None.

## Role Variables

    php_enablerepo: ""

(RedHat/CentOS only) If you have enabled any additional repositories (might I suggest [geerlingguy.repo-epel](https://github.com/geerlingguy/ansible-role-repo-epel) or [geerlingguy.repo-remi](https://github.com/geerlingguy/ansible-role-repo-remi)), those repositories can be listed under this variable (e.g. `remi-php70,epel`). This can be handy, as an example, if you want to install the latest version of PHP 7.0 with the latest version of phpMyAdmin, which is in the Remi repository.

    phpmyadmin_config_file: /etc/phpmyadmin/config.inc.php

The path to the phpMyAdmin config file. Platform specific values are defined in role vars directory.

    phpmyadmin_use_default_config: true

Whether to use the included default phpMyAdmin config file, defined with the following variables. Set this to `false` and copy your own `config.inc.php` file into the `phpmyadmin_config_path` if you'd like to use a more complicated setup. If this variable is set to `true`, all other configuration will be taken from phpMyAdmin's own [default config](https://docs.phpmyadmin.net/en/latest/config.html).

Available variables are listed below, along with default values (see `defaults/main.yml`):

    phpmyadmin_mysql_host: localhost
    phpmyadmin_mysql_port: ""
    phpmyadmin_mysql_socket: ""
    phpmyadmin_mysql_connect_type: tcp

These variables define the connection method and hostname phpMyAdmin will use to connect to the MySQL server.

    phpmyadmin_mysql_user: root
    phpmyadmin_mysql_password: "{{ mysql_root_password }}"

The username and password with which phpMyAdmin will attempt to log into the MySQL server. The `mysql_root_password` should be set as part of the `geerlingguy.mysql` role, but you can change the user and password to another account entirely, and you most defintely *should*, especially if you're connecting to a non-development database server!

    phpmyadmin_apache_template_path: phpmyadmin.conf.j2

Path to a template for generating an Apache proxy config file. Defaults to the template inside `templates/phpmyadmin.conf.j2`. This path should be relative to the directory from which you run your playbook.

    phpmyadmin_allow_access_ip:
      - 127.0.0.1
      - ::1
    phpmyadmin_allow_setup_ip:
      - 127.0.0.1
      - ::1

These variables define the IP addresses allowed to access phpmyadmin root and setup sites defined in the Apache phpmyadmin.conf files (can use a list of values). Default values for both variables are the localhost addresses.


## Dependencies

  - geerlingguy.apache
  - geerlingguy.mysql
  - geerlingguy.php
  - geerlingguy.php-mysql

## Example Playbook

    - hosts: utility
      vars_files:
        - vars/main.yml
      roles:
        - { role: geerlingguy.phpmyadmin }

*Inside `vars/main.yml`*:

    phpmyadmin_mysql_user: special_user
    phpmyadmin_mysql_password: secure_password_here

## TODO

  - Make default configuration more flexible (not everyone wants phpmyadmin to autologin as root).

## License

MIT / BSD

## Author Information

This role was created in 2014 by [Jeff Geerling](https://www.jeffgeerling.com/), author of [Ansible for DevOps](https://www.ansiblefordevops.com/).
