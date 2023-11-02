# ansible-rails

<img align="left" src="https://user-images.githubusercontent.com/759811/210273710-b13913e2-0a71-4d9d-94da-1fe538b8a73e.gif"/>

<br/>

 &nbsp;**Would you take a quick second and ⭐️ my repo?**

<br/>

This project is an Ansible playbook for provisioning and deploying a Rails/MySQL app to an Ubuntu server.  It is intended to be added to an existing Rails application folder.  Tested with: Rails 5.1.5, Ruby 2.4.3, Ubuntu 16.04 (Xenial).

## Demo

[![asciicast](https://asciinema.org/a/166682.png)](https://asciinema.org/a/166682)

## Setup

1. [Ansible](http://docs.ansible.com/ansible/latest/intro_installation.html) >= 2.4.1 must be installed
1. This playbook assumes you are targeting MySQL for your production database and have `gem 'mysql2'` included in your Gemfile.
1. From your Rails application folder run:
   
    ```shell
    \curl -sSL https://raw.githubusercontent.com/bradymholt/ansible-rails/master/installer.sh | bash
    ```
 
   which will:
  
   1. Add this project to a new folder in your app called "ops"
   2. Initialize the config file
   3. Add 2 new rake tasks to your Rakefile: `provision` and `deploy`.

   If you would prefer, you can run the commands in this file manually.
1. Edit the `ops/config.yml` file and add your project configuration

## Provisioning

Provisioning is used to to setup the the server and initially deploy the application.

To provision your server, run: `rake provision`.  This will do the following:

- Install the following:
  - RVM
  - Nginx
  - Phusion passenger
  - MySQL
  - Libraries: libxslt-dev, libxml2-dev, libmysqlclient-dev, imagemagick
- Create a user with ssh access and sudo authorization
- Setup a daily backup job to backup MySQL database to S3
- Create an app directory with appropriate permissions where Nginx config is pointing to
- Configure TLS (https) via Let's Encrypt
- Define an environment variable named `SECRET_KEY_BASE` with a unique uuid value.
- Deploy the application:
  - Precompile assets locally with `rake assets:precompile`.
  - Copy app files to to remote server in the deploy directory
  - Update `config/database.yml` on remote server with correct production db config
  - Run the following remotely:
  - `bundle install`
  - `rake db:migrate RAILS_ENV="production"`
  - `rake tmp:clear`
  - `rake log:clear`
  - `touch tmp/restart.txt` (restart the app)

## Deploying

If you have already provisioned your server and want to redeploy changes to your Rails app, run `rake deploy`.  This will only run the deploy tasks from the playbook and be much faster.
