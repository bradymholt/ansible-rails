# ansible-rails

Ansible playbooks for provisioning and depoying to a Rails webserver

## Setup

1. Run `ansible-galaxy install rvm_io.ruby`
2. Clone this repository as subfolder called `ansible` under your Rails app folder.
3. Create a file named `production` (no extension) at the root of the `ansible` folder with the IP address of the server to provision.  There is a `production.example` file in this repository that can serve as a template for what the file should look like.
4. Create a file named `production` (no extension) in the `groups_vars` folder with the various settings needed for provisioning.  There is a `production.example` file in this repository that can server as a templae for what the file should look like.

## Provisioning
To provision a new server:
`cd ansible && ansible-playbook -i production provision.yml`

This will:
- Install the following:
 - RVM
 - Nginx
 - Phusion passenger
 - MySQL
 - Libraries: libxslt-dev, libxml2-dev, libmysqlclient-dev, imagemagick
- Create user (defined by `deploy_user` var in [group_vars file](group_vars/production.)) with ssh access and sudo authorization
- Setup a daily backup job to backup MySQL database to S3 (config in [group_vars](https://github.com/bradyholt/ansible-rails/blob/master/group_vars/production.example#L6-L8) file)
- Create a deploy directory with appropriate permissions and where Nginx config is pointing to


## Deploying
To deploy the app:
`cd ansible && ansible-playbook -i production deploy.yml`

This will:
- Precompile assets locally: `rake assets:precompile`
- Copy files to to remote server in the deploy directory
- Update `config/database.yml` on remote server with correct production db config
- Run the following remotely:
 - `bundle install`
 - `rake db:migrate RAILS_ENV="production"`
 - `rake tmp:clear`
 - `rake log:clear`
 - `touch tmp/restart.txt` (restart the app)
