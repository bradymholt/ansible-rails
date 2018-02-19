# ansible-rails

This project is an Ansible playbook for provisioning and deploying a Rails/MySQL app to an Ubuntu server.  It is intended to be added to an existing Rails application folder.

This playbook has been tested with:

- Rails 5.1.5
- Ruby 2.4.3
- Ubuntu 16.04 (Xenial)

## Setup

1. [Ansible](http://docs.ansible.com/ansible/latest/intro_installation.html) >= 2.4.1 must be installed
1. This playbook assumes you are targeting MySQL for your production database and have `gem 'mysql2'` included in your Gemfile.
1. From your Rails application folder run the follwoing to add this project to a new folder in your app called "ops" and initialize the config file:
```
git clone https://github.com/bradymholt/ansible-rails.git ops && \
rm -rf ./ops/.git && \
mv ops/config.yml.example ops/config.yml
```
4. Edit the `ops/config.yml` file and add your project configuration
5. Add the following to your `Rakefile`:
```
task :provision do
  sh "ansible-playbook -i ./ops/config.yml ./ops/playbook.yml"
end

task :deploy do
  sh "ansible-playbook -i ./ops/config.yml ./ops/playbook.yml --tags 'deploy'"
end
```

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
