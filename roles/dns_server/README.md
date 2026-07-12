# Role: DNS Server
Sets up and deploys DNS server.
Current setup deploys Adguard Home as the default DNS server, but any can be used

## Tags
- core
- dns

## Required Variables
Below is an annotated breakdown of the required variables and their structure for this role

These blocks should be defined in the `host_vars` file for each host as all variables are unique to the specific host.

> [!WARNING]
> Any variable marked `# !SENSITIVE` **should not** be stored in plain text under any circumstance

### DNS Info
Defines settings for dns server quadlet to be deployed

```yaml
# (required) List of all DNS containers to be deployed
# in theory you can run multiple containers, however it is highly recommend to only run one
# at least one service is required
dns_services:

    # (required) name of the container in podman, and service name in systemd
    # This is unique to both podman and systemd
    # format: snake_case
  - name:

    # (required) URL of container image
    # can be from any container registry
    # format: url
    image:

    # (optional) category of the container
    # this changes the folder the quadlet is saved to.
    # default: "services"
    # format: string
    category: services

    # (optional) list of volume mounts to be used by the container
    # it is highly recommended to use volume mounts for DNS containers to persist configs
    # format example is listed below
    # default: []
    # format: /local/path/:/container/path
    volumes:
      - /local/path:/container/path
```

## Templates

### adguard_quadlet.j2
This template is used to generate .container files for DNS services. Currently it is set up to expect the use of Adguard Home container images.

## Execution
> [!CAUTION]
> This role targets and can potentially restart core services. As such it is recommended to only run this role when 100% necessary 

To run this command without running all roles in the playbook, use the following command
```bash
ansible-playbook site.yaml --tags "dns"
```
