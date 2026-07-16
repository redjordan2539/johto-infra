# Role: ntfy Server
This role sets up, starts and test the ntfy container.
This role is designed to only create one ntfy container per host. 

## Tags
- ntfy
- comms

## Required Variables
Below is an annotated breakdown of the required variables and their structure for this role

> [!WARNING]
> Any variable marked `# !SENSITIVE` **should not** be stored in plain text under any circumstance

### ntfy User Info
Defines users to be set up and used in ntfy.
These settings should be defined on each server you plan on setting up ntfy on as they are unique to each server

```yaml
# (required) username of the primary web user of the ntfy webui
# this account is used during the role to send a test post request to verify the server is running
# this username must also be defined in the below ntfy_users list
# format: username
ntfy_web_user:

# (required) password of the above web user
# this is used during basic auth to send the test request to the server
# !SENSITIVE
# format: string
ntfy_web_pass:

# (required) list of users to be created in the ntfy instance
# you must defined the above ntfy_web_user in this list
ntfy_users:
  # (required) username of the user to be created
  # format: username
  - username:
    # (required) password hash for the user
    # for more info on the requirements of this hash please read below
    # !SENSITIVE
    # format: string
    pass:

    # (required) user level of the generated user
    # you must define at least one admin user per instance
    # format: choice("user", "admin")
    level:
```
### ntfy container Info
Defines settings for the ntfy container

```yaml
# (optional) name of the ntfy container
# default: ntfy
# format: snake_case
ntfy_name:

# (optional) path to ntfy container image
# can be set to any container register
# default: docker.io/binwiederhier/ntfy
# format: URL
ntfy_image:

# (optional) name of the container owner
# typically set to the same user as the ansible user
# default: {{ ansible_user }}
# format: username
container_owner:

# (optional) flag to enable traefik labels in the container
# default: true
# format: bool
ntfy_enable_traefik:

# (optional) subdomain of the ntfy instance
# only used if `ntfy_enable_traefik` == true
# default: ntfy
# format: string
ntfy_subdomain:

# (optional) port to ntfy webui
# only used if `ntfy_enable_traefik` == true
# default: 80
# format: int
ntfy_port:

# (optional) podman network to bind ntfy container to
# default: management-net
# format: string
ntfy_network:

# (optional) URL of the ntfy webui
# default: ntfy.delpilar.net
# format: url
ntfy_url:
```
## Templates

### ntfy.container.j2
This template is used to generate .container files for ntfy services. This template includes a section for traefik labels.

## Execution
To run this role without running all other roles use the following command
```bash
ansible-playbook site.yaml --tags "ntfy"
```
