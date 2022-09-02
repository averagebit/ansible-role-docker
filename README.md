# docker (Ansible Role)

[![CI](https://github.com/averagebit/ansible-role-docker/workflows/CI/badge.svg?event=push)](https://github.com/averagebit/ansible-role-docker/actions?query=workflow%3ACI)

## Description

Installs and configures [Docker](https://www.docker.com/).

## Requirements

The role was developed and tested with the following Ansible versions.

| Name                                                   | Version     |
| ------------------------------------------------------ | ----------- |
| [ansible](https://pypi.org/project/ansible-base/)      | `>= 2.9.13` |
| [ansible-base](https://pypi.org/project/ansible-base/) | `>= 2.10.1` |
| [ansible-core](https://pypi.org/project/ansible-core/) | `>= 2.11.2` |

## Platforms

The role was tested on the following distributions and releases.

| Name   | Version |
| ------ | ------- |
| Ubuntu | `jammy` |

## Installation

`ansible-galaxy install averagebit.docker` will install the latest
stable release.

`ansible-galaxy install -r requirements.yml` will install the role
from a requirements file.

```yaml
# requirements.yml
---
roles:
  - name: averagebit.docker
    version: 1.0.0
```

## Variables

- `docker_edition`
  - Default: `"ce"`
  - Description: The available editions as of the moment of writing are
    `ce` (Community Edition) and `ee` (Enterprise Edition).
- `docker_packages`
  - Default: `["docker-{{ docker_edition }}", "docker-{{ docker_edition }}-cli", "{{ docker-{{ docker_edition }}-rootless-extras", "containerd.io"]`
  - Description: The Docker packages.
- `docker_packages_state`
  - Default: `"present"`
  - Description: Whether the `docker_packages` should be `"present"` on
    or `"absent"` from the system.
- `docker_compose_install`
  - Default: `false`
  - Description: Whether to install Docker Compose.
- `docker_compose_packages`
  - Default: `["docker-compose-plugin"]`
  - Description: The Docker Compose packages.
- `docker_compose_packages_state`
  - Default: `"present"`
  - Description: Whether the `docker_compose_packages` should be
    `"present"` on or `"absent"` from the system.
- `docker_architecture_map`
  - Default: `{"aarch": "arm64", "aarch64": "arm64", "amd64": "amd64", "x86_64": "amd64", "armhf": "armhf", "armv7l": "armhf", "s390x": "s390x"}`
  - Description: Used by the `docker_apt_architecture` variable to
    assign the correct architecture name based on the Docker repository
    naming conventions.
- `docker_debian_old_versions`
  - Default: `["docker", "docker-engine", "docker.io", "containerd", "runc"]`
  - Description: Old Docker packages which will be removed from the
    system.
- `docker_debian_dependencies`
  - Default: `["ca-certificates", "curl", "gnupg", "lsb-release"]`
  - Description: Docker Debian dependencies.
- `docker_debian_dependencies_state`
  - Default: `"present"`
  - Description: Whether the `docker_debian_dependencies` should be
    `"present"` on or `"absent"` from the system.
- `docker_repo_url`
  - Default: `"https://download.docker.com/linux"`
  - Description: The Docker repository URL.
- `docker_apt_architecture`
  - Default: `"docker_architecture_map[ansible_architecture]"`
  - Description: Assigns the correct architecture name based on the
    Docker repository naming conventions.
- `docker_apt_channel`
  - Default: `"stable"`
  - Description: As of writing the available channels are `"stable"` and
    `"nightly"`.
- `docker_apt_key_id`
  - Default: `"9DC858229FC7DD38854AE2D88D81803C0EBFCD88"`
  - Description: The Docker GPG key fingerprint, in case it has changed
    the new one can be obtained via `curl -fsSL https://download.docker.com/linux/ubuntu/gpg | gpg`.
- `docker_apt_key_url`
  - Default: `"{{ docker_repo_url }}/{{ ansible_distribution | lower }}/gpg"`
  - Description: The Docker GPG key URL.
- `docker_apt_key_state`
  - Default: `"present"`
  - Description: Whether the Docker GPG key should be `"present"` on or
    `"absent"` from the system.
- `docker_apt_repo`
  - Default: `"deb [arch={{ docker_apt_architecture }}] {{ docker_repo_url }}/{{ ansible_distribution | lower }} {{ ansible_distribution_release }} {{ docker_apt_channel }}"`
  - Description: The Docker apt repository.
- `docker_apt_repo_state`
  - Default: `"present"`
  - Description: Whether the `docker_apt_repo` should be `"present"` on
    or `"absent"` from the system.
- `docker_service_manage`
  - Default: `true`
  - Description: Whether to run the handlers responsible for managing
    the state of the service at the end of all tasks.
- `docker_service_state_running`
  - Default: `"started"`
  - Description: Whether the Docker service should be `"started"` or
    `"stopped"`.
- `docker_service_state_enabled`
  - Default: `true`
  - Description: Whether the Docker system service should start on boot.
- `docker_config`
  - Default: `{}`
  - Description: A dict of Docker configuration
    [options](https://docs.docker.com/engine/reference/commandline/dockerd/#daemon-configuration-file).
- `docker_config_mode`
  - Default: `0644`
  - Description: The permissions with which the `/etc/docker/daemon.js`
    file will be created.
- `docker_users`
  - Default: `[]`
  - Description: The users which should be added to the `docker` group.

## Usage

```yaml
# playbook.yml
- hosts: servers
  roles:
    - role: averagebit.docker
      become: true # required unless specified at the playbooks' top level
      tags: docker # (optional) convenience tag
  vars:
    docker_compose_install: true
    docker_users: ["foo"]
    docker_config:
      default-address-pools:
        - base: 172.18.0.0/16
          size: 24
```

## Legal

Copyright 2022 averagebit <[averagebit@pm.me](mailto:averagebit@pm.me)>

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
