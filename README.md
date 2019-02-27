Nginx Loadbalancer with SSL Self-Signed Certificate
===================================================

[![Build Status](https://travis-ci.org/in2it-ansible/nginx_loadbalancer_ssl.svg?branch=master)](https://travis-ci.org/in2it-ansible/nginx_loadbalancer_ssl)

This role adds SSL security on an existing load balancing reverse proxy using [Nginx](https://www.nginx.org).

Requirements
------------

To use this role, two packages are required to be installed on the target:

- openssl
- python-openssl

Role Variables
--------------

- **ssl_cert_path:** The base path for the certificates (default: /etc/ssl/private)
- **ssl_cert_cipher:** The cipher used for the certificate (default: aes256)
- **ssl_cert_size:** The bit-size of the certificate (default: 2048)
- **ssl_cert_provider:** The type of provider used by Ansible (default: selfsigned)
- **ssl_cert_file:** The name of the certificate (default: server.crt)
- **ssl_csr_key:** The name of the CSR (default: server.csr)
- **ssl_pub_key:** The name of the public key (default: server.pub.pem)
- **ssl_priv_key:** The name of the private key (default: server.priv.pem)
- **ssl_priv_pass:** A password for the private key -> ansbilbe_vault (default: "S3crE7!")

- **cert_org_name:** Your organisational name (default: Ansible)
- **cert_country:** Your country (default: FR)
- **cert_email:** Your e-mail address (default: jdoe@ansible.com)
- **cert_common_name:** The name of the web server (default: "{{ server_hostname }}")

- **cert_pass_file:** The location of your private key password **UNSECURE!!!** (default: /etc/nginx/server.pass)

- **nginx_service_user:** The owner of the certificates (default: nginx)
- **nginx_service_group:** The group of the certificates (default: nginx)

- **server_hostname:** The hostname Nginx listens to (default: lb.example.com)

Dependencies
------------

- [dragonbe.nginx_loadbalancer](https://github.com/in2it-ansible/nginx_loadbalancer)

Example Inventory
-----------------

    [all]
    lb ansible_host=192.168.1.1 
    web1 ansible_host=192.168.2.1 
    web2 ansible_host=192.168.2.2
    
    [lb]
    lb
    
    [web]
    web1
    web2

Example Playbook
----------------

    - name: Provision boxes
      hosts: all
      become: true
      roles:
        - { role: all, tags: [ 'common', 'all' ] }
    
    - name: Set up the web server
      hosts: web
      become: true
      roles: 
        - { role: dragonbe.nginx_fcgi, tags: [ 'nginx', 'web', 'fcgi' ] }
    
    - name: Setup load balancer
      hosts:
        - lb
      become: true
      roles:
        - { role: dragonbe.nginx_loadbalancer_ssl, tags: ['lb', 'nginx', 'web', 'ssl' ] }

License
-------

MIT

Author Information
------------------

Michelangelo van Dam (michelangelo+github@in2it.be)
