# duounix

This is a fairly uncomplicated role for installing Duo Security's
[duo_unix](https://duo.com/docs/duounix) on a system, to protect things like SSH
or console access. We intentionally **don't** handle things like PAM
configuration in this role, because those will vary widely by use case.

# Configuration

**Bold** items are required.

| Variable                      | Default                                                       | Notes                                     |
| --------                      | -------                                                       | -----                                     |
| **duounix_integration_key**   | None                                                          |                                           |
| **duounix_secret_key**        | None                                                          |                                           |
| **duounix_api_hostname**      | None                                                          |                                           |
| duounix_install               | package                                                       | could be `package` or `source`            |
| duounix_conf_dir              | /etc/duo                                                      |                                           |
| duounix_login_config          | `{failmode: secure, pushinfo: yes, autopush: no, prompts: 3}` | any extra config you want for login_duo   |
| duounix_pam_config            | Whatever is set in duounix_login_config                       | any extra config you want for pam_duo     |
| duounix_build_pam             | yes                                                           |                                           |

If building from source, the following variables take effect:

| Variable                      | Default                                                       | Notes                                     |
| --------                      | -------                                                       | -----                                     |
| duounix_checksum              | 2eb11dff0a173c62e31...                                        | SHA256 hash of the file                   |
| duounix_path                  | https://dl.duosecurity.com/                                   |                                           |
| duounix_version               | latest                                                        |                                           |
| duounix_prefix_dir            | /usr                                                          |                                           |

Pam configuration using Ansible templates via two variables (with their
defaults below):

duounix_pam_root_dir: /etc/pam.d/
duounix_pam_config_files: []

Example of duounix_pam_config_files populated.

duounix_pam_config_files:
  - name: sshd
    source: pam_ssh_config.template
  - name: example
    source: sub_dir/path/some_file.templ

Services are NOT restarted following pam configuration; consider using a
post_tasks entry in your playbook if you'd like it automated.

