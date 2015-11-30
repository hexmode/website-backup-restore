Role Name
=========

This role restores a [Semantic MediaWikis](http://www.semantic-mediawiki.org) instance using [Duplicity](http://duplicity.nongnu.org/) by executing the following steps:

	1. Create the dedicated OS user (matching the original DB user) for executing the SMW restore process
	2. Create the dedicated database to import the SMW database backup .sql into
	3. Create the dedicated SMW root to copy the restored SMW root content into
	3. Use [Duplicity](http://duplicity.nongnu.org/) to restore a backup set to /<OS user>/restore/
	4. Copy the restored SMW root content into the dedicated SMW root created in step 3
	5. Import the restored SMW database backup .sql into the database created in step 2
	6. Run the following SMW maintenance scripts:
		- maintenance/runJobs.php
		- maintenance/rebuildall.php
		- extensions/SemanticMediaWiki/maintenance/rebuildData.php
		- extensions/Cargo/maintenance/cargoRecreateData.php (if installed)

Requirements
------------

This role requires:

	- [Duplicity](http://duplicity.nongnu.org/)
	- [GnuPG](https://www.gnupg.org/)


Restore Configurations
----------------------

In templates/ configure your restore script. This script will then be referenced in "variables.yml" (see next point).

Note that the following variables are NOT set in this script but in the file "variables.yml" (see next point):

	- {{lsdr_restore_os_user}}
	- {{lsdr_restore_db_name}}

Role Variables
--------------

In accordance with defaults/main.yml please set all custom variables in the file "variables.yml". Then encrypt this file executing `ansible-vault encrypt variables.yml` and set a strong password.

Then include this file in your Vagrantfile by `ansible.extra_vars = "variables.yml"`. You will then either:

	- be prompted for this strong password each time you execute this role or
	- you store the strong password in a file called ansible.vault_password_file and include this file in your Vagrantfile by `ansible.extra_vars = "ansible.vault_password_file"`.

Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

Example Playbook
----------------

	---

	- hosts: all
	  sudo: True
	  gather_facts: False
	  roles:
	    - LinuxCompetenceCenter-Ansible-Roles.lcc-smw-duplicity-restore

License
-------

GNU GENERAL PUBLIC LICENSE

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).