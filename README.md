Trellis Monit
=========

This role is made to be used with [Trellis](https://roots.io/trellis/).
It configure Monit on the server and allow you to monitor any services.


Get Started
----------------
Add the role to the `galaxy.yml` file of Trellis :
```yaml
- name: trellis-monit
  src: 2kloc.trellis-monit
  version: 1.1.1
```

Run `ansible-galaxy install -r galaxy.yml` to install the new role.<br>
Then, add the role into server.yml :
```yaml
roles:
    ... other Trellis roles ...
    - { role: trellis-monit, tags: [monit]}
```

By default, the role will copy all monit configuration file located in the
`monit` folder at the root of the Trellis project.

```
monit
│   nginx.conf.j2
│   mysql.conf.j2
│   memcached.conf.j2
│   php7.1-fpm.conf.j2
│   ...
```

Here a really basic example for the Nginx service
```
check process nginx with pidfile /var/run/nginx.pid
    start program = "/etc/init.d/nginx start"
    stop program  = "/etc/init.d/nginx stop"
    group www-data
```
Check out the [Monit documentation](https://mmonit.com/wiki/Monit/ConfigurationExamples) for other examples.

Role Variables
--------------
You can use these variables to configure Monit like you want.
The variables could be add into `group_vars/all/main.yml`

```yaml

# Check service every X seconds
monit_daemon: 120

# Delay the first service check
monit_start_delay: false

# Path of monit files
monit_log_file: /var/log/monit.log
monit_pid_file: /var/run/monit.pid
monit_id_file: /var/lib/monit/id
monit_state_file: /var/lib/monit/state

# Event queue setup (Use if mail server not available)
monit_event_queue: true
monit_event_queue_file: /var/lib/monit/events
monit_event_queue_limit: 100

# Active mail server for alert delivery
monit_smtp: false

# If you active the mail server, you will need to define the sysadmin email
# with `monit_smtp_sysadmin_email` variable

# The role will use the Trellis mail variable by default but you can overwrite it
# monit_smtp_host: "mail.bar.baz"
# monit_smtp_port: "25"
# monit_smtp_username: "user"
# vault_monit_smtp_password: "password"
# monit_smtp_hostname: "mail.bar"
# monit_smtp_sender_address: "admin@mail.bar"

# You can avoid getting alerts with this filter
# monit_alert_filters:
#   - instance
#   - action
monit_alert_filters: []

# Enable Web Interface
monit_httpd: false

# Set the port for the Web Interface
monit_httpd_port: 2812

# Set the ssl directory for the Web Interface
monit_ssl_directory_name: "{{ nginx_path }}/ssl/monit"

# Set the credentiels for the Web Interface
monit_httpd_username: admin
vault_monit_httpd_password: monit

# Create a Nginx configuration for the web interface
monit_httpd_nginx: true

# Set the server_name for Nginx configuration
monit_httpd_server_name: host.example.com
monit_httpd_location: /monit/

# Set the folder with services monitoring files
monit_configuration_path: monit

# Use to delete all files missing a corresponding template in your local machine's
monit_configuration_cleanup: true
```