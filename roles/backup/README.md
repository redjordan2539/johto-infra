# Role: Backup
Installs and configures rclone and restic to handle daily backups
Creates systemd services for both daily restic backups and weekly restic prune jobs, and creates systemd timers to automatically run them

## Tags
- backup
- common

## Required Variables
Below is an annotated break down of the required variables and their structure for this role

These blocks should be defined in the `host_vars` file for each host as all variables are unique to the specific host.

> [!WARNING]
> Any variable marked `# !SENSITIVE` **should not** be stored in plain text under any circumstance

### Rclone Remote Info
This info is used to create the rclone.conf file.

All variables listed below are required for this role to run correctly
```yaml
# (required) This is a list of all rclone remotes to define in the rclone.conf
# At least one remote is required, but you may have as many as you want
rclone_remotes:
    # (required) Name of the remote to add. Must be unique to the host
    # format: string
  - remote_name: server-gdrive

    # (required) Client ID of google cloud project OAuth Client
    # format: string
    # !SENSITIVE
    client_id: 

    # (required) Client secret of google cloud project OAuth Client
    # format: string
    # !SENSITIVE
    client_secret: 

    # (required) Client auth token. This must be generated one time for each host
    # format: json string
    # !SENSITIVE
    client_auth_token: 

    # (required) ID for google drive shared drive. Found in the URL of the shared drive
    # https://drive.google.com/drive/u/0/folders/<Drive ID is Here>
    # format: string
    # !SENSITIVE
    team_drive_id: 
```

### Restic Repo Info
This info is used to create the restic service files, and defines what directories will be backed up.

All variables listed below are required for this role to run correctly

```yaml

# (required) password to restic repo
# This is required for any operations done to the restic repo. Keep this password saved somewhere secure that you can access
# format: string
# !SENSITIVE
restic_password:

# (required) name of the restic repo
# This name can be anything you want, however it must be unique per rclone remote
# This will also be the name of the folder in google drive
# format: snake_case
backup_name:

# (required) unique identifier for the repo. It is used by restic to differentiate each repo
# format: rclone:<rclone remote name>:{{ backup_name }}
restic_repo_url:

# (required) path to password file. 
# format: /path/to/file
restic_password_file:

# (required) path to rclone.conf file
# format: /path/to/rclone.conf
rclone_config_path:

# (required) list of paths to backup
# recommended paths to backup are listed in this example
# format: /path/to/dir
backup_paths:
  - /home
  - /etc
  - /var/log

# (required) number of daily backups to keep
# recommend value listed
# format: int
keep_daily: 7

# (required) number of weekly backups to keep
# recommended value listed
# format: int
keep_weekly: 4
```

## Execution
To run this role without running the entire playbook, use the following command:
```bash
ansible-playbook site.yaml --tags "backup"
```