# Ansible role LEMP
[![CI Molecule](https://github.com/darexsu/ansible-role-lemp/actions/workflows/ci.yml/badge.svg)](https://github.com/darexsu/ansible-role-lemp/actions/workflows/ci.yml)&emsp;![](https://img.shields.io/static/v1?label=idempotence&message=ok&color=success)&emsp;![Ansible Role](https://img.shields.io/ansible/role/d/59267?color=blue&label=downloads)

  - Role:
      - [platforms](#platforms)
      - [install](#install)
      - [requirements](#requirements)
      - [Merge behaviour](#merge-behaviour)
  - Playbooks (merge version):
      - [install and configure: LEMP-stack, (Nginx, MariaDB v10.5, PHP v7.4, Firewalld) ](#install-and-configure-lemp-with-mariadb-merge-version)
      - [install and configure: LEMP-stack, (Nginx, MySQL v8.0, PHP v7.4, Firewalld) ](#install-and-configure-lemp-with-mysql-merge-version)
        - [install: LEMP-stack, (Nginx, MariaDB v10.5, PHP v7.4) ](#install-lemp-with-mariadb-merge-version)
        - [install: LEMP-stack, (Nginx, MySQL v8.0, PHP v7.4) ](#install-lemp-with-mysql-merge-version)
  - Playbooks (full version):
      - [install and configure: LEMP-stack, (Nginx, MariaDB v10.5, PHP v7.4, Firewalld) ](#install-and-configure-lemp-with-mariadb-full-version)
      - [install and configure: LEMP-stack, (Nginx, MySQL v8.0, PHP v7.4, Firewalld) ](#install-and-configure-lemp-with-mysql-full-version)
        - [install: LEMP-stack, (Nginx, MariaDB v10.5, PHP v7.4) ](#install-lemp-with-mariadb-full-version)
        - [install: LEMP-stack, (Nginx, MySQL v8.0, PHP v7.4) ](#install-lemp-with-mysql-full-version)

### Platforms

|  Platforms       | Testing            |
| :--------------: | :----------------: |
| Debian 11        | :heavy_check_mark: |
| Debian 10        | :heavy_check_mark: |
| Ubuntu 20.04     | :heavy_check_mark: |
| Ubuntu 18.04     | :heavy_check_mark: |
| Oracle Linux 8   | :heavy_check_mark: |
| Rocky Linux 8    | :heavy_check_mark: |

### Install

```
ansible-galaxy install darexsu.lemp --force
```
### Requirements

roles: [Nginx](https://github.com/darexsu/ansible-role-nginx), [MariaDB](https://github.com/darexsu/ansible-role-mariadb), [MySQL](https://github.com/darexsu/ansible-role-mysql), [PHP](https://github.com/darexsu/ansible-role-php), [FirewallD](https://github.com/darexsu/ansible-role-firewalld) (will automatically be installed)

### Merge behaviour

Replace or Merge dictionaries (with "hash_behaviour=replace" in ansible.cfg):
```
# Replace             # Merge
---                   ---
  vars:                 vars:
    dict:                 merge:
      a: "value"            dict: 
      b: "value"              a: "value" 
                              b: "value"

# How does merge work?
Your vars [host_vars]  -->  default vars [current role] --> default vars [include role]
  
  dict:          dict:              dict:
    a: "1" -->     a: "1"    -->      a: "1"
                   b: "2"    -->      b: "2"
                                      c: "3"
    
```
##### Install and configure: LEMP with MariaDB (merge version)
```yaml
---
- hosts: all
  become: true

  vars:
    merge:
      # LEMP
      lemp:
        enabled: true
        domen: "example.site"
        project_dir: "/usr/share/nginx/html/"

      # Nginx
      nginx:
        enabled: true
      # Nginx -> install
      nginx_install:
        enabled: true
      # Nginx -> config -> nginx.conf
      nginx_conf:
        enabled: true
      # Nginx -> config -> {virtualhost}.conf
      nginx_virtualhost:
        default_conf:
          enabled: true

      # MariaDB
      mariadb:
        enabled: true
      mariadb_install:
        enabled: true

      # PHP
      php:
        enabled: true
        version: "7.4"
      # PHP -> install
      php_install:
        enabled: true
        modules: [common, fpm]
      # PHP -> config -> php.fpm
      php_fpm:
        www_conf:
          enabled: true

      # FirewallD
      firewalld:
        enabled: true
      # FirewallD -> rules
      firewalld_rules:
        service_http:
          enabled: true
        service_https:
          enabled: true


  tasks:
    - name: role darexsu lemp
      include_role:
        name: darexsu.lemp
```
##### Install and configure: LEMP with MySQL (merge version)
```yaml
---
- hosts: all
  become: true

  vars:
    merge:
      # LEMP
      lemp:
        enabled: true
        domen: "example.site"
        project_dir: "/usr/share/nginx/html/"

      # Nginx
      nginx:
        enabled: true
      # Nginx -> install
      nginx_install:
        enabled: true
      # Nginx -> config -> nginx.conf
      nginx_conf:
        enabled: true
      # Nginx -> config -> {virtualhost}.conf
      nginx_virtualhost:
        default_conf:
          enabled: true

      # MySQL
      mysql:
        enabled: true
      # MySQL -> install
      mysql_install:
        enabled: true

      # PHP
      php:
        enabled: true
        version: "7.4"
      # PHP -> install
      php_install:
        enabled: true
        modules: [common, fpm]
      # PHP -> config -> php.fpm
      php_fpm:
        www_conf:
          enabled: true

      # FirewallD
      firewalld:
        enabled: true
      # FirewallD -> rules
      firewalld_rules:
        service_http:
          enabled: true
        service_https:
          enabled: true

  tasks:
    - name: role darexsu lemp
      include_role:
        name: darexsu.lemp
```
##### Install: LEMP with MariaDB (merge version)
```yaml
---
- hosts: all
  become: true

  vars:
    merge:
      # LEMP
      lemp:
        enabled: true

      # Nginx
      nginx:
        enabled: true
      # Nginx -> install
      nginx_install:
        enabled: true

      # MariaDB
      mariadb:
        enabled: true
      # MariaDB -> install
      mariadb_install:
        enabled: true

      # PHP
      php:
        enabled: true
        version: "7.4"
      # PHP -> install
      php_install:
        enabled: true
        modules: [common, fpm]

  tasks:
    - name: role darexsu lemp
      include_role:
        name: darexsu.lemp
```
##### Install: LEMP with MySQL (merge version)
```yaml
---
- hosts: all
  become: true

  vars:
    merge:
      # LEMP
      lemp:
        enabled: true

      # Nginx
      nginx:
        enabled: true
      # Nginx -> install
      nginx_install:
        enabled: true

      # MySQL
      mysql:
        enabled: true
      # MySQL -> install
      mysql_install:
        enabled: true

      # PHP
      php:
        enabled: true
        version: "7.4"
      # PHP -> install
      php_install:
        enabled: true
        modules: [common, fpm]

  tasks:
    - name: role darexsu lemp
      include_role:
        name: darexsu.lemp
```
##### Install and configure: LEMP with MariaDB (full version)
```yaml
---
- hosts: all
  become: true

  vars:
    # LEMP
    lemp:
      enabled: true
      domen: "example.site"
      project_dir: "/usr/share/nginx/html/"

    # Nginx
    nginx:
      enabled: true
      repo: "nginx"
      service:
        enabled: true
        state: "started"
    # Nginx -> install
    nginx_install:
      enabled: true
    # Nginx -> config -> nginx.conf
    nginx_conf:
      enabled: true
      file: "nginx.conf"
      src: "nginx_conf.j2"
      backup: false
      data:
        user: "www-data"
        worker_processes: "auto"
        error_log: "/var/log/nginx/error.log notice"
        pidfile: "/var/run/nginx.pid"
        worker_connections: "1024"
        multi_accept: "off"
        mime_file_path: "/etc/nginx/mime.types"
        access_log: "/var/log/nginx/access.log"
        sendfile: "off"
        tcp_nopush: "off"
        tcp_nodelay: "on"
        keepalive_timeout: "75s"
        keepalive_requests: "1000"
    # Nginx -> config -> {virtualhost}.conf
    nginx_virtualhost:
      default_conf:
        enabled: true
        file: "default.conf"
        state: "present"
        src: "nginx_virtualhost.j2"
        backup: false
        data:
          listen_port: "80"
          listen_ipv6: false
          server_name: "{{ lemp.domen }}"
          root: "{{ lemp.project_dir }}"
          index: "index.html index.htm index.php"
          error_page: ""
          access_log: false
          error_log: false
          tcp_ip_socket:
            enabled: false
            listen: "127.0.0.1:9000"
          unix_socket:
            enabled: true
            file: "php{{ php.version }}-fpm.sock"

    # MariaDB
    mariadb:
      enabled: true
      repo: "mariadb"
      version: "10.5"
      service:
        state: "started"
        enabled: true
    # MariaDB -> install
    mariadb_install:
      enabled: true

    # PHP
    php:
      enabled: true
      version: "7.4"
      repo: "third_party"
      service:
        enabled: true
        state: "started"
    # PHP -> install
    php_install:
      enabled: true
      modules: [common, fpm]
    # PHP -> config -> php.fpm
    php_fpm:
      www_conf:
        enabled: true
        file: "www.conf"
        state: "present"
        src: "php_fpm.j2"
        backup: false
        data:
          webserver_user: "www-data"
          webserver_group: "www-data"
          pm: "dynamic"
          pm_max_children: "10"
          pm_start_servers: "5"
          pm_min_spare_servers: "5"
          pm_max_spare_servers: "5"
          pm_max_requests: "500"
          tcp_ip_socket:
            enabled: false
            listen: "127.0.0.1:9000"
          unix_socket:
            enabled: true
            file: "php{{ php.version }}-fpm.sock"
            user: "www-data"
            group: "www-data"

  tasks:
    - name: role darexsu lemp
      include_role:
        name: darexsu.lemp
```
##### Install and configure: LEMP with MySQL (full version)
```yaml
---
- hosts: all
  become: true

  vars:
    # LEMP
    lemp:
      enabled: true
      domen: "example.site"
      project_dir: "/usr/share/nginx/html/"

    # Nginx
    nginx:
      enabled: true
      repo: "nginx"
      service:
        enabled: true
        state: "started"
    # Nginx -> install
    nginx_install:
      enabled: true
    # Nginx -> config -> nginx.conf
    nginx_conf:
      enabled: true
      file: "nginx.conf"
      src: "nginx_conf.j2"
      backup: false
      data:
        user: "www-data"
        worker_processes: "auto"
        error_log: "/var/log/nginx/error.log notice"
        pidfile: "/var/run/nginx.pid"
        worker_connections: "1024"
        multi_accept: "off"
        mime_file_path: "/etc/nginx/mime.types"
        access_log: "/var/log/nginx/access.log"
        sendfile: "off"
        tcp_nopush: "off"
        tcp_nodelay: "on"
        keepalive_timeout: "75s"
        keepalive_requests: "1000"
    # Nginx -> config -> {virtualhost}.conf
    nginx_virtualhost:
      default_conf:
        enabled: true
        file: "default.conf"
        state: "present"
        src: "nginx_virtualhost.j2"
        backup: false
        data:
          listen_port: "80"
          listen_ipv6: false
          server_name: "{{ lemp.domen }}"
          root: "{{ lemp.project_dir }}"
          index: "index.html index.htm index.php"
          error_page: ""
          access_log: false
          error_log: false
          tcp_ip_socket:
            enabled: false
            listen: "127.0.0.1:9000"
          unix_socket:
            enabled: true
            file: "php{{ php.version }}-fpm.sock"

    # MySQL
    mysql:
      enabled: true
      repo: "mysql"
      version: "8.0"
      service:
        state: "started"
        enabled: true
    # MySQL -> install
    mysql_install:
      enabled: true

    # PHP
    php:
      enabled: true
      version: "7.4"
      repo: "third_party"
      service:
        enabled: true
        state: "started"
    # PHP -> install
    php_install:
      enabled: true
      modules: [common, fpm]
    # PHP -> config -> php.fpm
    php_fpm:
      www_conf:
        enabled: true
        file: "www.conf"
        state: "present"
        src: "php_fpm.j2"
        backup: false
        data:
          webserver_user: "www-data"
          webserver_group: "www-data"
          pm: "dynamic"
          pm_max_children: "10"
          pm_start_servers: "5"
          pm_min_spare_servers: "5"
          pm_max_spare_servers: "5"
          pm_max_requests: "500"
          tcp_ip_socket:
            enabled: false
            listen: "127.0.0.1:9000"
          unix_socket:
            enabled: true
            file: "php{{ php.version }}-fpm.sock"
            user: "www-data"
            group: "www-data"

    # FirewallD
    firewalld:
      enabled: true
      service:
        enabled: true
        state: "started"

    # FirewallD -> rules
    firewalld_rules:
      service_http:
        enabled: true
        zone: "public"
        state: "enabled"
        service: "http"
        permanent: true
        immediate: true
      service_https:
        enabled: true
        zone: "public"
        state: "enabled"
        service: "https"
        permanent: true
        immediate: true

  tasks:
    - name: role darexsu lemp
      include_role:
        name: darexsu.lemp
```
##### Install: LEMP with MariaDB (full version)
```yaml
---
- hosts: all
  become: true

  vars:
    # LEMP
    lemp:
      enabled: true
      domen: "example.site"
      project_dir: "/usr/share/nginx/html/"

    # Nginx
    nginx:
      enabled: true
      repo: "nginx"
      service:
        enabled: true
        state: "started"
    # Nginx -> install
    nginx_install:
      enabled: true

    # MariaDB
    mariadb:
      enabled: true
      repo: "mariadb"
      version: "10.5"
      service:
        state: "started"
        enabled: true
    # MariaDB -> install
    mariadb_install:
      enabled: true

    # PHP
    php:
      enabled: true
      version: "7.4"
      repo: "third_party"
      service:
        enabled: true
        state: "started"
    # PHP -> install
    php_install:
      enabled: true
      modules: [common, fpm]

  tasks:
    - name: role darexsu lemp
      include_role:
        name: darexsu.lemp
```
##### Install: LEMP with MySQL (full version)
```yaml
---
- hosts: all
  become: true

  vars:
    # LEMP
    lemp:
      enabled: true
      domen: "example.site"
      project_dir: "/usr/share/nginx/html/"

    # Nginx
    nginx:
      enabled: true
      repo: "nginx"
      service:
        enabled: true
        state: "started"
    # Nginx -> install
    nginx_install:
      enabled: true

    # MySQL
    mysql:
      enabled: true
      repo: "mysql"
      version: "8.0"
      service:
        state: "started"
        enabled: true
    # MySQL -> install
    mysql_install:
      enabled: true

    # PHP
    php:
      enabled: true
      version: "7.4"
      repo: "third_party"
      service:
        enabled: true
        state: "started"
    # PHP -> install
    php_install:
      enabled: true
      modules: [common, fpm]

  tasks:
    - name: role darexsu lemp
      include_role:
        name: darexsu.lemp
```
