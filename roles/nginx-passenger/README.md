nginx-passenger
=========

Ansible role for installing nginx with passenger on Debian/Ubuntu. It does the following:

1. Remove apache2, nginx and passenger
2. Install Passenger PGP key
3. Add HTTPS support for apt
4. Add passenger source to /etc/apt/sources.list.d/ depending on distro
5. apt-get update
6. Install nginx-extras and passenger
7. Set passenger_root and passenger_ruby
8. Restart nginx

This role *ONLY* ensures that nginx and passenger are installed and configed correctly. You need to config nginx to deploy your app. 

Requirements
------------

1. You *should* have ruby installed at `/usr/bin/ruby`. `passenger_ruby` will point to `/usr/bin/ruby`
2. You need `sudo: yes` to play the role.

Role Variables
--------------

NONE

Dependencies
------------

NONE

Example Playbook
----------------

```
- hosts: servers
  sudo: yes
  roles:
     - role: ChengLong.nginx-passenger 
```

License
-------

WTFPL

Author Information
------------------

[Cheng Long](https://twitter.com/ChengLong_)
