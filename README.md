# Ansible Role for Moodle

[![build](https://github.com/geoffreyvanwyk/ansible-role-moodle/workflows/Build/badge.svg?event=push)](https://github.com/geoffreyvanwyk/ansible-role-moodle/actions?query=workflow%3ABuild)

Deploys, installs, and upgrades [Moodle](https://moodle.org) on [Ubuntu](https://ubuntu.com) servers.

Additionally, extends Moodle by installing plugins from Git.

## Requirements

> Any pre-requisites that may not be covered by Ansible itself or the role
> should be mentioned here. For instance, if the role uses the EC2 module, it may
> be a good idea to mention in this section that the boto package is required.

The role is only tested on long-term support versions of Ubuntu that still
receive standard support.

At the moment, the role only uses Apache web server which it installs itself.

## Role Variables

> A description of the settable variables for this role should go here,
> including any variables that are in defaults/main.yml, vars/main.yml, and any
> variables that can/should be set via parameters to the role. Any variables that
> are read from other roles and/or the global scope (ie. hostvars, group vars,
> etc.) should be mentioned here as well.

### Deployment of Source Code

```yaml
moodle_deploy_repository: https://github.com/moodle/moodle # Public repository.
moodle_deploy_version: MOODLE_403_STABLE # Branch, tag, or commit.
moodle_deploy_update: yes # Whether to pull new commits.
moodle_deploy_destination: /var/www/html/moodle # Also the web document root.
```

The Moodle source code can only be deployed from a public repository and only
over HTTPS. For stable versions of Moodle, the associated Git branch can be
specified, e.g. for version 4.3, branch `MOODLE_403_STABlE` is used. Whenever
the role is run and new commits are available on the specified branch, variable
`moodle_deploy_update` specifies whether those new commits should be pulled
down. If `yes`, the Moodle instance will also be updated. The directory to
which the Moodle source code is deployed `moodle_deploy_destination` will also
the directory from which the web server will serve it.

---

### PHP

```yaml
php_version: 8.1
```

The PHP version is automatically set based on the `moodle_version`.

---

### Set-Up of Database

```yaml
moodle_db_install: yes      # Whether or not to install a database service.
moodle_cfg_dbtype: pgsql    # Which database service to use: pgsql or maridb.
moodle_cfg_dbname: moodle   # The name of the database.
moodle_cfg_dbuser: moodler
moodle_cfg_dbpass: *****    # A randomly-created password.
```

If a database service is installed, the specified user `moodle_cfg_dbuser` will have all administrative privileges on the specified database. If the database password `moodle_cfg_dbpass` is not specified, a random 24-character password consisting of ASCII letters and digits will be generated.

---

### Web Hosting

```yaml
moodle_web_service: apache2 # Or nginx, but only apache is currently supported.
moodle_web_root: subdirectory # of existing virtualhost or root of new virtualhost.
moodle_web_protected_directories: ... # See defaults/main.yml for default list.
moodle_web_protected_files: ... # See defaults/main.yml for default list.
moodle_web_protocol: http # Whether Moodle is served over 'https' or just 'http'.
moodle_web_domain: 127.0.0.1 # Domain name of the web service virtual host.
moodle_web_path: moodle # Subdirectory relative to virtual host.
```

Variable `moodle_web_root` specifies whether Moodle is served from a subdirectory of an existing virtual host or whether a new virtual host must be created for it specifically. If `subdirectory` is specified, then `moodle_deploy_destination` and `moodle_web_path` must be in accordance with it.

---

### Installation & Server-Side configuration

```yaml
moodle_environment: production
moodle_cfg_wwwroot: ... # Calculated based on web protocol, domain and path.
moodle_cfg_dataroot: ... # Calculated based on web domain and path.
moodle_site_fullname: Modular Object-Orientated Dynamic Learning Environment
moodle_site_shortname: Moodle
moodle_site_summary: >
  Moodle is the world’s most customisable and trusted eLearning solution that
  empowers educators to improve our world.

moodle_admin_username: moodler
moodle_admin_password: N3verstople@rning
moodle_admin_email: moodler@never.stops.learning
moodle_support_email: "{{ moodle_admin_email }}"
```

---

### Plugins

```yaml
moodle_plugins_git: []
```

A list of plugins that must be installed from a Git repository, because they
are not available in the official plugins directory. For each plugin, the
following must be specified:

- frankenstyle name of the plugin, e.g. mod_questionnaire, block_xp.
- URL to the repository;
- the version reference (that is the branch, tag or commit).

Additional plugins that are already installed, but that are not in this list,
will be uninstalled.

## Dependencies

> A list of other roles hosted on Galaxy should go here, plus any details in
> regards to parameters that may need to be set for other roles, or variables that
> are used from other roles.

The list of roles on which this role depends can be found in `requirements.yml` .

## Example Playbook

> Including an example of how to use your role (for instance, with variables
> passed in as parameters) is always nice for users too:

```yaml
- hosts: servers
  roles:
    - role: geoffreyvanwyk.moodle
      moodle_plugins_git:
        - name: theme_learningsandboxonline
          repository: https://github.com/geoffreyvanwyk/moodle-theme_learningsandboxonline
          version: MOODLE_403_STABLE
```

## License

Copyright &copy; 2023 Geoffrey Bernardo van Wyk [https://geoffreyvanwyk.dev](https://geoffreyvanwyk.dev)

This file is part of Ansible role geoffreyvanwyk.moodle.

Ansible role geoffreyvanwyk.moodle is free software: you can redistribute it
and/or modify it under the terms of the GNU General Public License as
published by the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

Ansible role geoffreyvanwyk.moodle is distributed in the hope that it will be
useful, but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU General
Public License for more details.

You should have received a copy of the GNU General Public License along with
Ansible role geoffreyvanwyk.moodle. If not, see <https://www.gnu.org/licenses/>.

## Author Information

> An optional section for the role authors to include contact information, or a
> website (HTML is not allowed).

[Geoffrey Bernardo van Wyk](https://geoffreyvanwyk.dev) created this role in 2023.
