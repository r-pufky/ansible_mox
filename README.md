# [Mox][h]
Mox Mail Server from Source.

## [Requirements][i]
Requires [r_pufky.srv][g] galaxy-ng collection. See
[additional documentation][m] and [reference documentation][h] for
troubleshooting and config variables.

* RAM: ~512MB
* Install size: ~170MB
* Data: ~15% metadata email overhead + email storage disk.

An understanding of how e-mail services work:

* [E-mail explained from first principles][p]
* [Hosting Mox mail server][q]
* [Configuring Mox mail server on Debian][r]

> Tasks [potentially touching Network Mounted Filesystems][o] will be run as
> the task user and fallback to the service user. Manage these locations
> externally if these fail (mox_flg_config=false and manually configure).
>
> Mounted filesystems that squash root permissions will fail (Mox executes
> certain file tasks as root and drops privileges to service user after opening
> network sockets; certain files use mox:root permissions).

## Role Variables
Detailed variable use documented in defaults. See usage for role operation.

* [defaults][j] - User configurable options.

* [ports][k] - Ports are **not** managed (defined for external use).

## Usage
Mox automatically manages file and directory permissions during startup. This
behavior can be disabled in Mox after initial configuration:

mox.conf
``` yaml
# Disable automatic perimission fix on startup.
NoFixPermissions: false
```

 Path                | Usage
 --------------------|-------
 /var/opt/mox        | Mox working directory, read/write area for service.
 /var/opt/mox/config | Mox configuration data.
 /var/opt/mox/data   | Mox runtime data (e.g. email).
 /var/opt/mox/web    | Mox static web (WebUI, ACME challenges).

### Feature Flags
Tasks are gated by feature flags and executed in the following order.

  Step | Flag            | Notes
 ------|-----------------|-------
  1    | mox_flg_fatal   | Halt execution if request version does not match role tested version.
  2    | mox_flg_install | Install required packages, users, etc.
  3    | mox_flg_config  | Install user-defined config.

### Example Playbooks

#### New Deployment
New Mox deployments require stepping through manual setup to correct configure
required certificates, etc. This will deploy a new Mox installation ready for
manual configuration.

``` yaml
- name: 'Deploy Mox. Service not started, Manual configuration required.'
  ansible.builtin.include_role:
    name: 'r_pufky.srv.mox'
```

Manually walk through mail configuration steps:
``` bash
ssh mail.example.com
su - mox -s /bin/bash
cd /var/opt/mox
mox --help
mox quickstart user@example.com
# Copy completed config to ansible controller for Pre-configured deployments.
...
systemctl start mox
```

#### Pre-configured deployment
Most existing Mox deployments will use this. Service automatically restarted
when configuration is defined.

``` yaml
- name: 'Deploy Mox server with static configuration.'
  ansible.builtin.include_role:
    name: 'r_pufky.srv.mox'
  vars:
    mox_flg_config: true
    mox_cfg_d: 'host_vars/mail.example.com/config'
```

## Management
Mox is designed to be managed interactively via the Binary.

``` bash
ssh mail.example.com
su - mox -s /bin/bash
cd /var/opt/mox
mox --help
```

## Development
Configure [environment][a].

``` bash
# Run all tests.
molecule test --all
```

### [Releases][b]

  Release | Debian | Ansible | Mox     | Notes
 ---------|--------|---------|---------|-------
  2.x.x   | 13     | 2.20    | v0.0.15 | Ansible 2.20, feature flags, and semantic versioning.
  1.x.x   | 13     | 2.18    | v0.0.15 | Initial release.

## Issues
Create a bug and provide as much information as possible.

Associate pull requests with a submitted bug.

## License
[AGPL-3.0 License][c] | [direct link][f]

## Author Information
PGP: [466EEC2B67516C7117C85CE3A0BC35D16698BAB9][d] | [github gist][e]

[a]: https://r-pufky.github.io/ansible_docs
[b]: https://semver.org/spec/v2.0.0
[c]: https://www.tldrlegal.com/license/gnu-affero-general-public-license-v3-agpl-3-0
[d]: https://keys.openpgp.org/vks/v1/by-fingerprint/466EEC2B67516C7117C85CE3A0BC35D16698BAB9
[e]: https://gist.github.com/r-pufky/a8df36977c55b5bb20829267c4c49d22

[f]: https://github.com/r-pufky/ansible_mox/blob/main/LICENSE
[g]: https://github.com/r-pufky/ansible_collection_srv
[h]: https://www.xmox.nl/config
[i]: https://github.com/r-pufky/ansible_mox/blob/main/meta/main.yml
[j]: https://github.com/r-pufky/ansible_mox/tree/main/defaults/main/main.yml
[k]: https://github.com/r-pufky/ansible_mox/blob/main/defaults/main/ports.yml
[m]: https://r-pufky.github.io/docs/mail/mox
[o]: https://r-pufky.github.io/ansible_docs/best_practice/patterns/#network-mounts
[p]: https://explained-from-first-principles.com/email
[q]: https://nich.dk/posts/hosting-my-own-mox-mail-server
[r]: https://community.hetzner.com/tutorials/install-and-configure-mailserver-mox-on-debian