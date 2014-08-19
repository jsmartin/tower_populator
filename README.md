# tower_populator

The purpose of this project is to populate an Ansible Tower installation from a configuration file.  

## Requirements

	pip install ansible-tower-cli 
	
**current version requires installation from tower-cli issue-9-unstable branch for dynamic group support**

make sure you have ```~/.tower_cli.cfg``` configured with something like:


	host: 10.42.0.200
	username: admin
	password: password

## Usage 
 
Usage is simple.  Declare the options you want in [config.yml](config.yml), and run the script:

```
./tower_populator config.yml

Creating Organization

Hyrule Ventures Mining Rupees Daily

Creating Users

{'username': 'link', 'is_superuser': True, 'first_name': 'Link', 'last_name': 'Smith', 'email': 'link@hyrule.com'}
{'username': 'gdorf', 'is_superuser': False, 'first_name': 'Gannon', 'last_name': 'Dorf', 'email': 'gannon@hyrule.com'}
{'username': 'zelda', 'is_superuser': False, 'first_name': 'Zelda', 'last_name': 'Smith', 'email': 'zelda@hyrule.com'}
{'username': 'epona', 'is_superuser': False, 'first_name': 'Epona', 'last_name': 'Horse', 'email': 'epona@hyrule.com'}
{'username': 'demise', 'is_superuser': False, 'first_name': 'De', 'last_name': 'Mise', 'email': 'demise@hyrule.com'}

Creating Teams

{'description': 'The Ops Team', 'users': ['link'], 'name': 'Ops'}
{'description': 'The QA Team', 'users': ['gdorf'], 'name': 'QA'}
{'description': 'The Dev Team', 'users': ['zelda'], 'name': 'Dev'}

Creating Credentials

{'username': 'vagrant', 'kind': 'ssh', 'name': 'Local SSH', 'team': 'Ops', 'private_key': '/root/vagrant-key', 'description': 'Used for non-cloud instances'}
{'username': 'myaccesskey', 'kind': 'aws', 'name': 'AWS creds', 'team': 'Ops', 'password': 'mysecretkey', 'description': 'Used for AWS'}
{'username': 'myusername', 'kind': 'rax', 'name': 'Rax Creds', 'team': 'Ops', 'password': 'myapikey', 'description': 'Used for Rackspace'}

Creating Inventories

{'name': 'Production', 'groups': [{'source': 'ec2', 'credential': 'AWS creds', 'description': 'EC2 hosts', 'name': 'EC2'}], 'description': 'Production Machines'}
{'name': 'Test', 'description': 'Test Machines'}
{'name': 'QA', 'description': 'QA Machines'}

Creating Projects

{'scm_type': 'git', 'name': 'Hyrulian Playbooks', 'scm_url': 'https://github.com/jsmartin/demo-cli', 'description': 'Configures all the servers in Hyrule.'}
{'scm_type': 'git', 'name': 'Ansible Examples', 'scm_url': 'https://github.com/ansible/ansible-examples', 'description': 'Some example roles and playbooks'}

Must sync projects from Tower UI if pulling from SCM.  Press any key after synch is finished.


Creating Job Templates

{'name': 'Apache', 'verbosity': 0, 'job_type': 'run', 'project': 'Hyrulian Playbooks', 'inventory': 'Production', 'forks': 7, 'machine_credential': 'Local SSH', 'playbook': 'apache.yml', 'description': 'Confgure Apache servers'}
{'name': 'Graphite', 'verbosity': 2, 'job_type': 'run', 'project': 'Hyrulian Playbooks', 'inventory': 'Production', 'machine_credential': 'Local SSH', 'playbook': 'graphite.yml', 'description': 'Confgure Graphite servers'}
```

## Gotchas

tower-cli doesn't do everything the Tower UI does (yet), so there are some limitations:

1. Projects must be synched manually from the Tower CLI if they are pulled from a SCM.  The script will prompt you for this.  There is a [RFE for this](https://github.com/ansible/tower-cli/issues/14).
2. All credential types use ```username``` and ```password``` as options, even AWS, Rackspace, etc.  This is a [known issue](https://github.com/ansible/tower-cli/issues/13).

