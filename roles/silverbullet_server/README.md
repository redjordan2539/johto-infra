# Role: SilverBullet Server
Sets up and deploys SilverBullet container
For more information about SilverBullet check out [their website](https://silverbullet.md/)

## Tags
- silverbullet
- docs

## Required Variables
Below is an annotated breakdown of the required variables and their structure for this role

> [!WARNING]
> Any variable marked `# !SENSITIVE` **should not** be stored in plain text under any circumstance

### SilverBullet Info
Defines settings for the SilverBullet Server.
Most of these variables are optional, and have predefined defaults in the role itself, however it is possible to define custom variables in `host_vars` files.
Any variable marked as optional has a default value defined in `defaults/main.yaml` for this role

```yaml
# (optional): Name of the silverbullet container
# This name is also the name of the service created by the quadlet file, 
# as well as the name of the .container file
# default: silverbullet
# format: snake_case
silverbullet_name:

# (optional): Image of the silverbullet container
# This can be any image from any register that podman supports
# default: ghcr.io/silverbulletmd/silverbullet:latest
# format: URL
silverbullet_image:

# (optional): Port of the silverbullet webui
# this is used for traefik labels to allow for access to the webui
# the default value is defined by the silverbullet container itself. For information on changing it, view the documentation for your image
# default: 3000
# format: int
silverbullet_port:

# (optional): Username of the silverbullet user
# this is the default user created for silverbullet
# note, this value has a default, but it is highly recommended to change it via `host_vars`
# default: jdelpilar
# format: username
silverbullet_user:

# (optional): password for the above created user
# note, this value has a default, but it is highly recommended to change it via `host_vars`
# !SENSITIVE
# default: generated password
# format: password
silverbullet_pass:

# (optional): url of the silverbullet instance
# this is used to set the url in traefik
# default: silverbullet.delpilar.net
# format: url
silverbullet_url:
```

## Templates

### silverbullet.container.j2
This template creates a `.container` file for silverbullet
By default this template supports the following additional variables not listed above.
- silver_bullet_volumes: used to define additional mounted volumes
  - format: <host_path>:<container_path>
- silver_bullet_env: used to define additional environment variables used by the container
  - format: dict(<key>:<value>)

## Execution
> [!NOTE]
> This role restarts any containers where the `.container` file is changed. As such, please ensure that all work is saved and all users are made aware of any potential downtime.

To run this role without running any other roles, please use this command
```bash
ansible-playbook site.yaml --tags silverbullet
```

To run all roles with the `docs` tag at once, please use this command
```bash
ansible-playbook site.yaml --tags docs
```
