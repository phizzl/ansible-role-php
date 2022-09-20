# Ansible role PHP

This is a role for installing and managing PHP cli and fpm.

## Vars

| Name                           | Description                                              | Default | Required |
|--------------------------------|----------------------------------------------------------|---|---|
| php_version                    | PHP version to install                                   | 8.1 | yes | 
| php_ini_global                 | Global settings for cli and fpm                          | / | no |
| php_ini_cli                    | Settings for cli                                         | / | no |
| php_ini_fpm                    | Settings for fpm (all pools)                             | / | no |
| php_default_extensions_enabled | Install a list of default extensions (see vars/main.yml) | true | no |
| php_extensions                 | List of own PHP extensions to be installed               | [] | no |
| php_set_alternative            | Use this installation as update alternative              | true | no |
| php_fpm_install                | Install FPM service                                      | true | no |
| php_fpm_enable                 | Enable FPM service                                       | true | no |
| php_fpm_allow_restart          | Allow role to restart FPM if necessary                   | true | no |
| php_fpm_error_log              | Location of FPM error log                                | /var/log/php_errors.log | no |
| php_fpm_error_log_level        | FPM log level                                            | warning | no |
| php_fpm_pools                  | List of FPM pools (see `php_fpm_pools` details)          | [] | no |
| php_composer_install           | Flag wether to install composer or not                   | true | no |
| php_composer_target            | Path to install composer                                 | /usr/local/bin/composer | no |

### php_fpm_pools details

| Name                   | Description                       | Default                                                  | Required |
|------------------------|-----------------------------------|----------------------------------------------------------|----------|
| name                   | Name of the pool                  | /                                                        | yes      | 
| listen                 | Listen (socket/interface)         | /                                                        | yes      |
| listen_owner           | Owner for the socket              | www-data                                                 | no |
| listen_group           | Group for the socket              | www-data                                                 | no |
| listen_mode            | Filemode for the socket           | "0660"                                                   | no |
| listen_allowed_clients | Allowed clients                   | 127.0.0.1                                                | no |
| user                   | Effective user for executing PHP  | www-data                                                 |
| group                  | Effective group for executing PHP | www-data                                                 |
| error_log              | Path for error log                | /                                                        | no       |
| slow_log               | Path for slow log                 | /                                                        | no       |
| environment            | Environment vars for FPM          | {}                                                       | no       |
| admin_values           | Admin values to set for pool      | /                                                        | no       |
| extra_settings                       | Extra settings like `clear_env` for pool | see `default_php_fpm_pool_app_config` in `vars/main.yml` | no |

## Example config

Configure one pool
```yaml
php_fpm_pools:
  - name: www
    listen: /run/php/www.sock
    error_log: /var/log/php_fpm_www_errors.log
    slow_log: /var/log/php_fpm_www_slow.log
    extra_vars:
      clear_env: "no"
    environment:
      DATABASE_HOST: 10.0.0.1
```

Use the role
```yaml
- host: webserver
  become: yes
  roles:
    - role: phizzl.php
```
