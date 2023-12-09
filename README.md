[![ZoneMinder](https://zoneminder.com/images/care.png)](https://zoneminder.com/)

# Ansible Role - ZoneMinder
Ansible Role to deploy a ZoneMinder IP-CAM server.

Read into the [official documentation](https://zoneminder.readthedocs.io/en/stable/userguide/gettingstarted.html) on how to add ip-cams and so on.

[![Molecule Test Status](https://badges.ansibleguy.net/sw_zoneminder.molecule.svg)](https://github.com/ansibleguy/_meta_cicd/blob/latest/templates/usr/local/bin/cicd/molecule.sh.j2)
[![YamlLint Test Status](https://badges.ansibleguy.net/sw_zoneminder.yamllint.svg)](https://github.com/ansibleguy/_meta_cicd/blob/latest/templates/usr/local/bin/cicd/yamllint.sh.j2)
[![PyLint Test Status](https://badges.ansibleguy.net/sw_zoneminder.pylint.svg)](https://github.com/ansibleguy/_meta_cicd/blob/latest/templates/usr/local/bin/cicd/pylint.sh.j2)
[![Ansible-Lint Test Status](https://badges.ansibleguy.net/sw_zoneminder.ansiblelint.svg)](https://github.com/ansibleguy/_meta_cicd/blob/latest/templates/usr/local/bin/cicd/ansiblelint.sh.j2)
[![Ansible Galaxy](https://badges.ansibleguy.net/galaxy.badge.svg)](https://galaxy.ansible.com/ui/standalone/roles/ansibleguy/sw_zoneminder)

Molecule Logs: [Short](https://badges.ansibleguy.net/log/molecule_sw_zoneminder_test_short.log), [Full](https://badges.ansibleguy.net/log/molecule_sw_zoneminder_test.log)

**Tested:**
* Debian 11

## Install

```bash
# latest
ansible-galaxy role install git+https://github.com/ansibleguy/sw_zoneminder

# from galaxy
ansible-galaxy install ansibleguy.sw_zoneminder

# or to custom role-path
ansible-galaxy install ansibleguy.sw_zoneminder --roles-path ./roles

# install dependencies
ansible-galaxy install -r requirements.yml
```

## Functionality

* **Package installation**
  * ZoneMinder Server
    * Base package and dependencies
    * Apache2 => using [THIS Role](https://github.com/ansibleguy/infra_apache)
    * MariaDB => using [THIS Role](https://github.com/ansibleguy/infra_mariadb)


* **Configuration**
  * Default opt-ins:
    * Database setup
    * Webserver setup

  * Default opt-outs:
    * Admin-tools

  * Default config:
    * Logging to syslog
    * Self-Signed certificate

## Info

* **Note:** this role currently only supports debian-based systems


* **Note:** Most of the role's functionality can be opted in or out.

  For all available options - see the default-config located in the main defaults-file!


* **Warning:** You should AT LEAST [set a login password after the installation finished](https://zoneminder.readthedocs.io/en/stable/userguide/gettingstarted.html#enabling-authentication).


## Usage

### Config

Define the zoneminder dictionary as needed.

Example for a zoneminder server:
```yaml
zoneminder:
  timezone: 'Europe/Vienna'
  tools: true  # install useful admin-tools
  
  apache:
    domain: 'zoneminder.template.ansibleguy.net'
    aliases: ['zm.template.ansibleguy.net']

    ssl:
      mode: 'letsencrypt'  # or selfsigned/ca
      #  if you use 'selfsigned' or 'ca':
      #    cert:
      #      cn: 'ZoneMinder Server'
      #      org: 'AnsibleGuy'
      #      email: 'zoneminder@template.ansibleguy.net'
    letsencrypt:
      email: 'zoneminder@template.ansibleguy.net'
  
  
```

Bare minimum example:
```yaml
zoneminder:
  apache:
    domain: 'zoneminder.template.ansibleguy.net' 
```

You might want to use 'ansible-vault' to encrypt your passwords:
```bash
ansible-vault encrypt_string
```

### Execution

Run the playbook:
```bash
ansible-playbook -K -D -i inventory/hosts.yml playbook.yml --ask-vault-pass
```

There are also some useful **tags** available:
* config
