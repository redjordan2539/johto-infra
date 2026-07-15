# Role: Minecraft Server
Sets up and deploys minecraft server container

## Tags
- minecraft

## Required Variables
Below is an annotated breakdown of the required variables and their structure for this role

These blocks should be defined in the `host_vars` file for any host that should be a minecraft server

> [!WARNING]
> Any variable marked `# !SENSITIVE` **should not** be stored in plain text under any circumstance

### Minecraft Info
Defines settings for the Minecraft server container settings

```yaml
# (required) List of all minecraft server containers to be deployed
# you can run as many minecraft servers as the hardware can handle. However, all will need a unique port
# at least one service is required
minecraft_servers:
    # (required) name of the server container
    # all containers and services will be prefixed with 'mc-'
    # for example if you list 'vanilla' as the name the resulting container/service name will be 'mc-vanilla'
    # format: snake_case
  - name:
    # (required) URL of container image
    # can be from any container registry
    # recommended image is: docker.io/itzg/minecraft-server:latest
    # format: url
    image: docker.io/itzg/minecraft-server:latest
    # (optional) host port to bind to the container
    # this must be unique for each server instance
    # default: 25565
    # format: int
    port:
    # (optional) list of environment variables to pass to the container
    # these are primarily used to define settings for the minecraft server
    # for more info please read the docs for your specific container image
    # memory setting example listed to show proper format
    # format: key value pair
    env:
      MEMORY: 4G
```

## Templates

### minecraft.container.j2
This template is used to generate .container files for Minecraft servers. This template is heavily opinionated to be a minecraft server instance and should not be used a generic template for quadlets.

## Execution
> [!NOTE]
> If this role makes changes to the quadlet file on the host the server will be restarted. If you are hosting this server for multiple users please keep that in mind 

To run this command without running all roles in the playbook, use the following command
```bash
ansible-playbook site.yaml --tags "minecraft"
```
