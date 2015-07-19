# ansible-rails

Ansible playbooks for provisioning and depoying to a Rails webserver

## Setup
Clone this repository as subfolder called `ansible` under your Rails app folder.  

## Provisioning
To provision a new server, `cd` to the : 
`cd ansible && ansible-playbook -i production provision.yml`

## Deploying
To deploy the app: 
`cd ansible && ansible-playbook -i production deploy.yml`
