# Ansible Role: phpMyAdmin

[![Build Status](https://travis-ci.org/geerlingguy/ansible-role-phpmyadmin.svg?branch=master)](https://travis-ci.org/geerlingguy/ansible-role-phpmyadmin)

Installs phpMyAdmin on RHEL/CentOS/Debian/Ubuntu.

phpMyAdmin is a simple interface for interacting with MySQL databases via a web browser. It is not necessarily the most secure or efficient method of managing databases, but for those who need a GUI, this one is better than many others. I (geerlingguy) would personally never run it on a production server, nor do I use it myself (I use Sequel Pro or simply interact with the database via CLI/APIs), but it seems many people like it (especially people stuck on a Windows machine with no good MySQL GUIs :).

## Requirements

**RedHat/CentOS**: Requires the EPEL repository on RedHat/CentOS 6.x hosts. You can install the EPEL repository using the `geerlingguy.repo-epel` role.

**Debian/Ubuntu**: None.

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

    phpmyadmin_mysql_host: localhost
    phpmyadmin_mysql_port: ""
    phpmyadmin_mysql_socket: ""
    phpmyadmin_mysql_connect_type: tcp

These variables define the connection method and hostname phpMyAdmin will use to connect to the MySQL server.

    phpmyadmin_mysql_user: root
    phpmyadmin_mysql_password: "{{ mysql_root_password }}"

The username and password with which PHPMyAdmin will attempt to log into the MySQL server. The `mysql_root_password` should be set as part of the `geerlingguy.mysql` role, but you can change the user and password to another account entirely, and you most defintely *should*, especially if you're connecting to a non-development database server!

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

This role was created in 2014 by [Jeff Geerling](http://jeffgeerling.com/), author of [Ansible for DevOps](http://ansiblefordevops.com/).
