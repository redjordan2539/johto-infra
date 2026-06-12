# johto-infra
The core Ansible project for provisioning and managing the Del Pilar homelab infrastructure


## Summary
This contains the primary Ansible configuration-as-code to provision and manage my home infrastructure.
Currently this system manages server baselines, and container deployment for internal DNS (via AdguardHome) and git/ci (via Gitea) using Podman Quadlets

## Scope and Limitations
**Warning** This project is heavily opinionated and designed to work with one specific home lab architecture.

The roles, tasks and templates within this repo are best used as a reference for managing services running as podman quadlets.

If you want to run this repo against your own home lab you will need to rewrite 'inventory.yaml' and all group and host vars to match your specific environment.

## Repository Structure
Below is the directory map for this repo. 
```text
├── ansible.cfg
├── group_vars
│   └── all.yaml
├── host_vars
│   ├── goldenrod.yaml
│   └── new-bark.yaml
├── inventory.yaml
├── README.md
├── roles
│   ├── common
│   │   └── tasks
│   │       └── main.yaml
│   ├── dns_server
│   │   ├── handlers
│   │   │   └── main.yaml
│   │   ├── tasks
│   │   │   └── main.yaml
│   │   └── templates
│   │       └── adguard_quadlet.j2
│   └── gitea_server
│       ├── handlers
│       │   └── main.yaml
│       ├── tasks
│       │   └── main.yaml
│       └── templates
│           ├── act_runner_quadlet.j2
│           └── gitea_quadlet.j2
└── site.yaml
````

## Prerequisites

To run this project locally the following tools are required.
All install commands assume you are running Ubuntu/Debian.

- Ansible installed on the control machine
    ```bash
    sudo apt install ansible
    ```
- Ansible Lint (optional but highly recommended if making changes)
    ```bash
    sudo apt install ansible-lint
    ```
- SSH access to target nodes

## How to Run
The ultimate goal of this project is a fully automated gitops workflow. 
However, until that is fully running below are the instructions to run this project locally.

- Clone the repo
    ```bash
    git clone git@git.delpilar.net:jdelpilar/johto-infra.git
    cd johto-infra
    ```
- To run all plays in site.yaml and fully initialize the environment run the following command. This will target all nodes.
    ```bash
    ansible-playbook site.yaml
    ```
- To limit the execution to a single host use the limit flag (-l, --limit)
    ```bash
    # This will only target new-bark
    ansible-playbook site.yaml -l new-bark 
    ```
- To limit the execution to only a specific service or tag, use the tag flag (-t, --tags)
    ```bash
    # This will only run dns tasks, but will target all nodes
    ansible-playbook site.yaml -t dns 
    ```
- These flags can be combined if needed
    ```bash
    # This will only run dns tasks and only target new-bark
    ansible-playbook site.yaml -t dns -l new-bark
    ```

## Services Deployed
Below is a list of all services currently deployed by this project. This list will be updated as new services are added

Unless otherwise stated all services are run via rootless podman quadlets.

- Internal DNS (dns_server) - AdguardHome for network wide adblocking and local DNS resoultion
- Git Server and CI/CD runner (gitea_server) - Gitea alongside act runner for local git with repo mirroring and local private ci/cd runners
