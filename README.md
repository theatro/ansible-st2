# Ansible-st2-secure FORK
This fork is focused on providing security out-of-the-box for StackStorm. Original README follows.
Note that the original stackstorm role is include via git submodule, so make sure to use one of these:

```
git clone --recurse-submodules https://github.com/theatro/ansible-st2-secure.git
```
or
```
git clone https://github.com/theatro/ansible-st2-secure.git
cd ansible-st2-secure
git submodule init
git submodule update
```

# Ansible-st2
Ansible playbooks to deploy [StackStorm](https://github.com/stackstorm/st2).
> [StackStorm](http://stackstorm.com/) is event-driven automation platform written in Python.
With over [50+ integrations](https://github.com/StackStorm/st2contrib/tree/master/packs) like GitHub, Docker, Nagios, NewRelic, AWS, Ansible it allows you to wire together your existing infrastructure into complex Workflows with auto-remediation and many more.
Aka IFTTT orchestration for Ops.

[![Build Status](https://travis-ci.org/StackStorm/ansible-st2.svg?branch=master)](https://travis-ci.org/StackStorm/ansible-st2)
[![Repository deb/rpm](https://img.shields.io/badge/Repository-deb/rpm-blue.svg)](https://packagecloud.io/StackStorm/stable/)
[![Join our community Slack](https://stackstorm-community.herokuapp.com/badge.svg)](https://stackstorm.com/community-signup)

## Supported platforms
* Ubuntu Trusty (14.04)
* Ubuntu Xenial (16.04)
* RHEL6 / CentOS6
* RHEL7 / CentOS7

> If you're using the provided Vagrantfile, note that it uses Xenial by default.

## Requirements
At least 2GB of memory and 3.5GB of disk space is required, since StackStorm is shipped with RabbitMQ, PostgreSQL, Mongo, nginx and OpenStack Mistral.

## Installation
```sh
# stackstorm
ansible-playbook stackstorm.yml
```

## Variables
Below is the list of variables you can redefine in your playbook to customize st2 deployment:

| Variable                 | Default       | Description  |
| ------------------------ | ------------- | ------------ |
| **st2repo**
| `st2repo_name`           | `stable`      | StackStorm PackageCloud repository to install. [`stable`](https://packagecloud.io/StackStorm/stable/), [`unstable`](https://packagecloud.io/StackStorm/unstable/), [`staging-stable`](https://packagecloud.io/StackStorm/staging-stable/), [`staging-unstable`](https://packagecloud.io/StackStorm/staging-unstable/)
| **st2**
| `st2_version`            | `latest`      | StackStorm version to install. `present` to install available package, `latest` to get automatic updates, or pin it to numeric version like `2.2.0`.
| `st2_revision`           | `1`           | StackStorm revision to install. Used only with pinned `st2_version`.
| `st2_config`             | `{}`          | Hash with StackStorm configuration settings to set in [`st2.conf`](https://github.com/StackStorm/st2/blob/master/conf/st2.conf.sample) ini file.
| `st2_system_user`        | `stanley`     | System user from which st2 will execute local/remote shell actions.
| `st2_system_user_in_sudoers` | `yes`| Add `st2_system_user` to the sudoers (recommended for most `st2` features to work).
| `st2_ssh_key_file`       | `/home/{{st2_system_user}}/.ssh/{{st2_system_user}}_rsa` | Path to `st2_system_user` SSH private key. It will be autogenerated by default.
| `st2_auth_enable`        | `yes`         | Enable StackStorm standalone authentication.
| `st2_auth_username`      | `testu`       | Username used by StackStorm standalone authentication.
| `st2_auth_password`      | `testp`       | Password used by StackStorm standalone authentication.
| `st2_save_credentials`   | `yes`         | Save credentials for local CLI in `/root/.st2/config` file.
| **st2mistral**
| `st2mistral_version`     | `latest`      | st2mistral version to install. `present` to install available package, `latest` to get automatic updates, or pin it to numeric version like `2.2.0`.
| `st2mistral_db`          | `mistral`     | PostgreSQL DB name that will be created for Mistral.
| `st2mistral_db_username` | `mistral`     | PostgreSQL DB user that will be created for Mistral.
| `st2mistral_db_password` | `StackStorm`  | PostgreSQL DB password for Mistral.
| `st2mistral_config`      | `{}`          | Hash with configuration settings to set in [`mistral.conf`](https://github.com/StackStorm/st2-packages/blob/master/packages/st2mistral/conf/mistral.conf) ini file.
| **st2web**
| `st2web_ssl_certificate`     | `null` | String with custom SSL certificate (`.crt`). If not provided, self-signed certificate will be generated.
| `st2web_ssl_certificate_key` | `null` | String with custom SSL certificate secret key (`.key`). If not provided, self-signed certificate will be generated.
| **bwc**
| `bwc_license`            | `null`        | BWC license key is required for installing BWC enteprise bits via this ansible role.
| `bwc_repo`               | `enterprise`  | BWC PackageCloud repository to install. [`enterprise`](https://packagecloud.io/StackStorm/enterprise/), [`enterprise-unstable`](https://packagecloud.io/StackStorm/enterprise-unstable/), [`staging-enterprise`](https://packagecloud.io/StackStorm/staging-enteprise/), [`staging-enterprise-unstable`](https://packagecloud.io/StackStorm/staging-enterprise-unstable/)
| `bwc_version`            | `latest`      | BWC enterprise version to install. `present` to install available package, `latest` to get automatic updates, or pin it to numeric version like `2.2.0`. The version used here should match `st2_version`.
| `bwc_revision`           | `1`           | BWC enterprise revision to install. Used only with pinned `bwc_version`.
| `bwc_rbac` | [See `bwc_rbac` variable in role defaults](roles/bwc/defaults/main.yml) | BWC RBAC roles and assignments. This is a dictionary with two keys `roles` and `assignments`. `roles` and `assignments` are in turn both arrays. Each element in the array follows the exact YAML schema for [roles](https://bwc-docs.brocade.com/rbac.html#user-permissions) and [assignments](https://bwc-docs.brocade.com/rbac.html#defining-user-role-assignments) defined in BWC documentation.
| `bwc_ldap` | [See `bwc_ldap` variable in role defaults](roles/bwc/defaults/main.yml) | Settings for BWC LDAP authentication backend. `bwc_ldap` is a dictionary and has one item `backend_kwargs`. `backend_kwargs` should be provided as exactly listed in BWC documentation for [LDAP configuration](https://bwc-docs.brocade.com/authentication.html#auth-backends).
| **st2chatops**
| `st2chatops_version`     | `latest`      | st2chatops version to install. `present` to install available package, `latest` to get automatic updates, or pin it to numeric version like `2.2.0`.
| `st2chatops_st2_api_key` |               | st2 API key to be updated in st2chatops.env using "st2 apikey create -k" in a task
| `st2chatops_hubot_adapter` |             | Hubot Adapter to be used for st2chatops. Default is `shell`, but should be changed to one of the [`supported adapters`](`https://github.com/StackStorm/ansible-st2/blob/master/roles/st2chatops/vars/main.yml`).[**Required**]
| `st2chatops_config`      | `{ }`         | Based on adapter in `st2chatops_hubot_adapter`, provide hash for the adapter settings, to update [`st2chatops.env`](https://github.com/StackStorm/st2chatops/blob/master/st2chatops.env). For example, for `Slack` hubot adapter: `st2chatops_config:` `HUBOT_SLACK_TOKEN: xoxb-CHANGE-ME-PLEASE`
| `st2chatops_version`     | `latest`      | st2chatops version to install. Use `latest` to get automatic updates or pin it to numeric version like `2.2.0`.

## Examples
Install latest `stable` StackStorm with all its components on local machine:
```sh
ansible-playbook stackstorm.yml -i 'localhost,' --connection=local
```

> Note that keeping `latest` version is useful to update StackStorm by re-running playbook, since it will reinstall st2 if there is new version available.
This is default behavior. If you don't want updates - consider pinning version-revision numbers.

Install specific numeric version of st2 with pinned revision number as well:
```sh
ansible-playbook stackstorm.yml --extra-vars='st2_version=2.2.0 st2_revision=8'
```

## Installing behind a proxy.

If you are installing from behind a proxy, you can use environment variables `http_proxy`, `https_proxy`, and `no_proxy` in the playbook. For the
st2smoketests, you will need to disable proxy for localhost.

```yaml
  environment:
    http_proxy: http://proxy.example.net:3128
    https_proxy: http://proxy.example.net:3128
    no_proxy: 127.0.0.1,localhost
```

## Developing
There are a few requirements when developing on `ansible-st2`.

These are the platforms we must support (must pass end-to-end testing):
- Xenial
- Trusty
- CentOS6
- CentOS7
- RHEL6 (via AWS)
- RHEL7 (via AWS)

Must also support Ansible Idempotence (Eg. Ansible-playbook re-run should end with the following results: `changed=0.*failed=0`)

For development purposes there is [Vagrantfile](Vagrantfile) available. The following command will setup ubuntu16 box (`ubuntu/xenial64`) by default:
```sh
vagrant up
```

Other distros:
```sh
vagrant up ubuntu14
vagrant up centos6
vagrant up centos7
```

## Other Installers
You might be interested in other methods to deploy StackStorm engine:
* Configuration Management
  * [Chef Cookbook](https://github.com/StackStorm/chef-stackstorm/)
  * [Puppet Module](https://github.com/stackstorm/puppet-st2)

* Manual Instructions
  * [Ubuntu 14.04/16.04](https://docs.stackstorm.com/install/deb.html)
  * [RHEL7/CentOS7](https://docs.stackstorm.com/install/rhel7.html)
  * [RHEL6/CentOS6](https://docs.stackstorm.com/install/rhel6.html)

## Help
If you're in stuck, our community always ready to help, feel free to:
* Ask questions in our [public Slack channel](https://stackstorm.com/community-signup)
* [Report bug](https://github.com/StackStorm/ansible-st2/issues), provide [feature request](https://github.com/StackStorm/ansible-st2/pulls) or just give us a ✮ star

Your contribution is more than welcome!
