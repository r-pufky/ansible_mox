# Mox
Mox Mail Server.

## Requirements
[supported platforms](https://github.com/r-pufky/ansible_mox/blob/main/meta/main.yml)

Requires:
* **~512MB** RAM
* **~15%** metadata + email storage disk.

An understanding of how e-mail services work:
* https://explained-from-first-principles.com/email/
* https://nich.dk/posts/hosting-my-own-mox-mail-server/
* https://community.hetzner.com/tutorials/install-and-configure-mailserver-mox-on-debian

## Role Variables
[defaults](https://github.com/r-pufky/ansible_mox/tree/main/defaults/main)

### Ports
All ports and protocols have been defined for the role.

[defaults/ports.yml](https://github.com/r-pufky/ansible_mox/blob/main/defaults/main/ports.yml)

## Dependencies
**galaxy-ng** roles cannot be used independently. Part of
[r_pufky.srv](https://github.com/r-pufky/ansible_collection_srv) collection.

## Example Playbook
Read defaults documentation.

### Pre-configured deployment
Service will automatically be restarted when configuration is defined.

``` yaml
- name: 'Mox server'
  hosts: 'mail.example.com'
  become: true
  roles:
     - 'r_pufky.srv.mox'
  vars:
    mox_srv_import_config: 'host_vars/mail.example.com/data/config'
```

### New deployment
Deploy Mox requiring manual server configuration. Service will be installed but
not started as additional Mox configuration is **required**.
``` yaml
- name: 'Mox server'
  hosts: 'mail.example.com'
  become: true
  roles:
     - 'r_pufky.srv.mox'
```

Manually walk through mail configuration steps:
``` bash
ssh mail.example.com
su - mox -s /bin/bash
cd /data/mox
mox --help
...
systemctl start mox
```
Copy configuration to ansible controller.

## Management
Manage interactively via the binary.

``` bash
ssh mail.example.com
su - mox -s /bin/bash
cd /data/mox
mox --help
```

## Development
Configure [environment](https://github.com/r-pufky/ansible_collection_docs/blob/main/ansible/environment.md)

Run all unit tests:
``` bash
molecule test --all
```

### Releases
Release format: **{OS}-{SERVICE}-{ROLE}**

Each type inherits the versioning system used; defaulting to schematic
versioning.

`12.0.0-2.0.3-1.0.0`

* 12.0.0 - Debian 12 (bookworm).
* 2.0.3 - Service/app version.
* 1.0.0 - Role version.

Releases are branched on Debian releases:

* **[13.x.x](https://github.com/r-pufky/ansible_mox)**: 13 Trixie.
* **[12.x.x](https://github.com/r-pufky/ansible_mox/tree/12.x)**: 12 Bookworm.

## Issues
Create a bug and provide as much information as possible.

Associate pull requests with a submitted bug.

## License
[AGPL-3.0 License](https://www.tldrlegal.com/license/gnu-affero-general-public-license-v3-agpl-3-0)
 [(direct link)](https://github.com/r-pufky/ansible_mox/blob/main/LICENSE)

## Author Information
PGP Fingerprint: [466EEC2B67516C7117C85CE3A0BC35D16698BAB9](https://keys.openpgp.org/vks/v1/by-fingerprint/466EEC2B67516C7117C85CE3A0BC35D16698BAB9)
| [github gist](https://gist.github.com/r-pufky/a8df36977c55b5bb20829267c4c49d22)
