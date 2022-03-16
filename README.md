<p><img src="https://avatars0.githubusercontent.com/u/695951?s=200&v=4" alt="minio logo" title="minio" align="right" height="60" /></p>

# Ansible Role: MinIO

[![Build Status](https://travis-ci.org/atosatto/ansible-minio.svg?branch=master)](https://travis-ci.org/atosatto/ansible-minio)
[![License](https://img.shields.io/badge/license-MIT%20License-brightgreen.svg)](https://opensource.org/licenses/MIT)
[![Ansible Role](https://img.shields.io/badge/ansible%20role-atosatto.minio-blue.svg)](https://galaxy.ansible.com/atosatto/minio/)
[![GitHub tag](https://img.shields.io/github/tag/atosatto/ansible-minio.svg)](https://github.com/atosatto/ansible-minio/tags)

Install and configure the [MinIO](https://minio.io/) S3 compatible object storage server on RHEL/CentOS and Debian/Ubuntu.

## Requirements

None.

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

```yaml
minio_server_bin: /usr/local/bin/minio
minio_client_bin: /usr/local/bin/mc
```

Installation path of the MinIO server and client binaries.

```yaml
minio_server_release: "RELEASE.2022-02-26T02-54-46Z"
minio_client_release: "RELEASE.2022-02-26T03-58-31Z"
```

Release to install for both server and client; lastest if the default. Can be 'RELEASE.2022-02-26T02-54-46Z' for instance.

```yaml
minio_user: minio
minio_group: minio
```

Name and group of the user running the minio server.
**NB**: This role automatically creates the minio user and/or group if they do not exist in the system.

```yaml
minio_server_envfile: /etc/default/minio
```

You can specify the location of your existing config using --config-dir (default: ${HOME}/.minio)

```yaml
minio_config_dir: "/etc/minio"
```

Path to the file containing the minio server configuration ENV variables.

```yaml
minio_server_addr: ":9091"
```

The MinIO server listen address.

```yaml
minio_server_datadirs:
  - /var/lib/minio
```

Directories of the folder containing the minio server data

```yaml
minio_server_make_datadirs: true
```

Create directories from `minio_server_datadirs`

```yaml
minio_server_args: [ ]
```

Set a list of nodes to create a [distributed cluster](https://docs.minio.io/docs/distributed-minio-quickstart-guide).

In this mode, ansible will create your server datadirs, but use this list for the server startup. Note you will need a number of disks to satisfy MinIO's distributed storage requirements.

Example:

```yaml
minio_server_datadirs:
  - '/data1/minio'
  - '/data2/minio'

minio_server_args:
  - 'http://test-minio{1...4}.ciic-cloud.cn:9000/data{1...2}/minio'
```

Additional environment variables to be set in MinIO server environment

```yaml
minio_server_env_extra: |
  MINIO_REGION_NAME=us-east-1
  MINIO_API_REQUESTS_MAX=1600
  MINIO_SERVER_URL="http://test-minio.ciic-cloud.cn:9000"
```

Additional CLI options that must be appended to the minio server start command.

```yaml
minio_server_opts: ""
```


MinIO root user and password.

```yaml
minio_root_username: ""
minio_root_password: ""
```

Switches to disable minio server and/or minio client installation.
```yaml
minio_install_server: true
minio_install_client: true
```

## Dependencies

None.

## Example Playbook

```yaml
---
- name: "Install MinIO"
  hosts: all
  any_errors_fatal: true
  become: yes
  roles:
    - ansible-minio
  vars:
	  minio_access_key: "minioadmin"
    minio_secret_key: "CiiC@65613920MiniO"
	  minio_server_datadirs:
	    - '/data1/minio'
	    - '/data2/minio'
	  minio_server_args:
	    - 'http://test-minio{5...8}.ciic-cloud.cn:9000/data{1...2}/minio'
    minio_server_env_extra: |
      MINIO_API_REQUESTS_MAX=1600
      MINIO_SERVER_URL="http://test-minio.ciic-cloud.cn:9000"
```

## Changelog

See [changelog](CHANGELOG.md).

## License

MIT
