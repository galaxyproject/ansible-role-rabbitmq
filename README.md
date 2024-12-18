galaxyproject.rabbitmq
======================

[![Ansible Role](https://img.shields.io/ansible/role/60839)](https://galaxy.ansible.com/galaxyproject/rabbitmq)
[![Molecule Tests](https://github.com/galaxyproject/ansible-rabbitmq/actions/workflows/molecule.yml/badge.svg)](https://github.com/galaxyproject/ansible-rabbitmq/actions/workflows/molecule.yml)

galaxyproject.rabbitmq is an [Ansible][ansible] role for installing [RabbitMQ][rabbitmq] and managing its virtual hosts, users, and plugins.

To use Docker, see the [usegalaxy_eu.rabbitmqserver][eu_docker] role instead.

[ansible]: http://www.ansible.com
[rabbitmq]: https://www.rabbitmq.com/
[eu_docker]: https://github.com/usegalaxy_eu/ansible-rabbitmq/

Requirements
------------

None

Role Variables
--------------

See [defaults](defaults/main.yml) for a full list.

### Server Configuration

See:

- [RabbitMQ - Configuration](https://www.rabbitmq.com/docs/configure)

Only "New-style" configuration via [`rabbitmq.conf`](https://www.rabbitmq.com/docs/configure#config-file) is supported.

Set the `rabbitmq_config` variable to set configuration options.

```yaml
rabbitmq_config:
  listeners:
    tcp: none
  ssl_listeners:
    default: 5671
  ssl_options:
    cacertfile: /etc/ssl/certs/fullchain.pem
    certfile: /etc/ssl/certs/cert.pem
    keyfile: /etc/ssl/user/privkey-rabbitmq.pem
    verify: verify_peer
    fail_if_no_peer_cert: false
    versions:
      - tlsv1.3
      - tlsv1.2
```

### Users

See:

- [Ansible - RabbitMQ User Module](https://docs.ansible.com/ansible/latest/collections/community/rabbitmq/rabbitmq_user_module.html)
- [RabbitMQ - Access Control](https://www.rabbitmq.com/docs/access-control)

Set the `rabbitmq_users` variable to define an array of users. Parameters and defaults are described in the
rabbitmq_user module documentation.

```yaml
rabbitmq_users:
- user: guest
  state: absent
- user: admin
  password: admin
  tags: administrator
```

### Virtual Hosts

See:

- [Ansible - RabbitMQ Virtual Host Module](https://docs.ansible.com/ansible/latest/collections/community/rabbitmq/rabbitmq_vhost_module.html)
- [RabbitMQ - Virtual Hosts](https://www.rabbitmq.com/docs/vhosts)

Set the `rabbitmq_vhosts` variable to define an array of virtual hosts. Parameters and defaults are described in the
rabbitmq_vhost module documentation.

```yaml
rabbitmq_vhosts:
- name: /one
- name: /two
  node: rabbit
  tracing: no
- name: three
  state: absent
```

### Plugins

See:

- [Ansible - RabbitMQ Plugin Module](https://docs.ansible.com/ansible/latest/collections/community/rabbitmq/rabbitmq_plugin_module.html)
- [RabbitMQ - Plugins](https://www.rabbitmq.com/docs/plugins)

Set the `rabbitmq_plugins` variable to define an array of plugins. Parameters and defaults are described in the
rabbitmq_plugin module documentation.

```yaml
rabbitmq_plugins:
- names: rabbitmq_management
- names: rabbitmq_delayed_message_exchange
  state: disabled
```

### Cluster

See:

- [RabbitMQ - Clustering Guide](https://www.rabbitmq.com/docs/clustering)

Set the `rabbitmq_cluster` variable to enable clustering.

```yaml
rabbitmq_cluster: yes
```

Set the `rabbitmq_erlang_cookie` variable to define the Erlang cookie.

```yaml
rabbitmq_erlang_cookie: g9avtqdzdm2p5oe9
```

Dependencies
------------

- [community.rabbitmq](https://galaxy.ansible.com/community/rabbitmq/) collection

Example Playbook
----------------

```yaml
- name: RabbitMQ Servers
  hosts: rabbitmq_servers
  vars:
    rabbitmq_config:
      vm_memory_high_watermark:
        relative: 0.8
      disk_free_limit:
        absolute: 500MB
      listeners:
        tcp:
          default: 5672
    rabbitmq_users:
      - user: guest
        state: absent
      - user: admin
        password: admin
        tags: administrator
    rabbitmq_vhosts:
      - name: /one
  roles:
    - galaxyproject.rabbitmq
```

License
-------

[MIT](LICENSE)

Author Information
------------------

[Originally written by Jason Royle](https://github.com/jasonroyle/ansible-role-rabbitmq).

[Additional contributors](https://github.com/jasonroyle/ansible-role-rabbitmq/graphs/contributors).
