# Role: Common
Installs basic tools and configures base settings for all servers
This role should always be able to target all hosts.

## Tags
- common


## Required Variables
Below is an annotated break down of the required variables and their structure for this role
These variables can be defined either in `host_vars` or `group_vars` as they are generic enough that all hosts can have the same path. However it is recommended to define them per host if possible

> [!WARNING]
> Any variable marked `# !SENSITIVE` **should not** be stored in plain text under any circumstance

### Podman Variables
This information is used to setup base podman directories

All variables listed below are required for this role to run correctly

```yaml
# (required) path to directory where podman volume mounts will be placed
# format: /path/to/dir
podman_config_base_dir:

# (required) path to where quadlet files will be saved
# Recommended value listed below. Without changes, this is the only path the container generator will look for container files on systemd daemon-reload
# format: /path/to/dir
podman_quadlet_base_dir: "/home/{{ ansible_user }}/.config/containers/systemd"
```

## Execution
> [!NOTE]
> This role cannot be ran by itself as other required roles are marked with the common tag. This is by design

To run all common roles, use the following command
```bash
ansible-playbook site.yaml --tags "common"
```
