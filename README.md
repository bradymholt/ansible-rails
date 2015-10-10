# ansible-rails

Ansible playbooks for provisioning and depoying to a Rails webserver

## Setup

1. Run `ansible-galaxy install rvm_io.rvm1-ruby`
2. Clone this repository as subfolder called `ansible` under your Rails app folder.
3. Create a file named `production` (no extension) at the root of the `ansible` folder with the IP address of the server to provision.  There is a `production.example` file in this repository that can serve as a template for what the file should look like.
4. Create a file named `production` (no extension) in the `groups_vars` folder with the various settings needed for provisioning.  There is a `production.example` file in this repository that can server as a templae for what the file should look like.

## Provisioning
To provision a new server:
`cd ansible && ansible-playbook -i production provision.yml`

## Deploying
To deploy the app: 
`cd ansible && ansible-playbook -i production deploy.yml`
