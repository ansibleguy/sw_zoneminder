[![ZoneMinder](https://zoneminder.com/images/care.png)](https://zoneminder.com/)

# Ansible Role - ZoneMinder
Ansible Role to deploy a ZoneMinder IP-CAM server.

Read into the [official documentation](https://zoneminder.readthedocs.io/en/stable/userguide/gettingstarted.html) on how to add ip-cams and so on.

**Tested:**
* Debian 11


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

* **Warning:** THIS ROLE IS NOT YET IN A STABLE STATE!


* **Note:** this role currently only supports debian-based systems


* **Note:** Most of this functionality can be opted in or out using the main defaults file and variables!

## Setup
For this role to work - you must install its dependencies first:

```
ansible-galaxy install -r requirements.yml
```

## Usage

### Config

Define the zoneminder dictionary as needed.

Example for a zoneminder server:
```yaml
zoneminder:
  timezone: 'Europe/Vienna'
  tools: true  # install useful admin-tools'
  
  apache:
    domain: 'zoneminder.template.ansibleguy.net'
    aliases: ['zm.template.ansibleguy.net']

    ssl:
      mode: 'letsencrypt'  # or selfsigned
      #  if you use 'selfsigned':
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
* install
* db
