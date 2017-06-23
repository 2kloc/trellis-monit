Trellis Monit
=========

This role is made to be used with [Trellis](https://roots.io/trellis/).
It configure Monit on the server and allow you to monitor any services.


Get Started
----------------
Add the role to the requirements.yml file of Trellis :
```yaml
- name: trellis-monit
  src: 2kloc.trellis-monit
  version: 1.0.0
```

Run ansible-galaxy install -r requirements.yml to install the new role.
Then, add the roles to the server.yml :
```yaml
roles:
    ... other Trellis roles ...
    - { role: 2kloc.trellis-monit, tags: [monit]}
```

Role Variables
--------------
```yaml
monit_daemon: 120
monit_start_delay: false
monit_log_file: /var/log/monit.log
monit_pid_file: /var/run/monit.pid
monit_id_file: /var/lib/monit/id
monit_state_file: /var/lib/monit/state
monit_event_queue: true
monit_event_queue_file: /var/lib/monit/events
monit_event_queue_limit: 100
monit_smtp: false

# The role will use the Trellis mail variable by default but you can overwrite it
# monit_smtp_host: "mail.bar.baz"
# monit_smtp_port: "25"
# monit_smtp_username: "user"
# vault_monit_smtp_password: "password"
# monit_smtp_hostname: "mail.bar"
# monit_smtp_sender_address: "admin@mail.bar"

monit_alert_filters: []
monit_httpd: false
monit_httpd_nginx: true
monit_httpd_port: 2812
monit_httpd_username: admin
vault_monit_httpd_password: monit
monit_httpd_server_name: host.example.com
monit_httpd_location: /monit/
monit_configuration_path: monit
monit_configuration_pattern: "^({{ monit_configuration_path | regex_escape }})/(.*)\\.j2$"
monit_configuration_cleanup: true
```

License
-------

MIT

(C) [2KLOC](https://github.com/2koc) 2017.
------------------
