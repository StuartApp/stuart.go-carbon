Ansible Role for go-carbon
=========

This role will install and configure [go-carbon](https://github.com/lomik/go-carbon), a _Golang implementation of Graphite/Carbon server_

Requirements
------------

This role will only take care of the **carbon** component installation. You have to take care of disabling the original Carbon program in a default Graphite installation.

Role Variables
--------------

The whole go-carbon configuration is automatically generated based on the values of three dicts:

```yaml
go_carbon_conf:
  common:
    user: "carbon"
    max-cpu: 4
  whisper:
    data-dir: "/var/lib/graphite/whisper"
go_carbon_storage_schemas:
  default:
    pattern: ".*"
    retentions: "60s:30d,1h:5y"
go_carbon_storage_aggregation_rules:
  default:
    pattern: ".*"
    xFilesFactor: 0.5
    aggregationMethod: "average"
```

The first level will create a `[section]` in the corresponding file and all the other key/values will create a key/value entry in that section.

There's only one notable exception in `go_carbon_conf`, which is `logging`. Since go-carbon supports multiple loggers, you can define multiple entries like this:

```yaml
go_carbon_conf:
  logging:
    - logger: ""
      file: "/var/log/go-carbon/go-carbon.log"
      level: "info"
      encoding: "mixed"
      encoding-time: "iso8601"
      encoding-duration: "seconds"
      ## you can add more loggers here, they will appear as [[logging]] sections
      # - logger: ""
      #   file: "stderr"
      #   level: "error"
```

**Please note**: the default values are in `vars/main.yml` under `go_carbon_conf_defaults` but you must override them using the `go_carbon_conf` dict.

Dependencies
------------

There are no extra dependencies

Example Playbook
----------------

This will install go-carbon and customize its configuration, creating the necessary directories

```yaml
- hosts: servers
  roles:
- { role: stuart.go-carbon,
    vars: {
      go_carbon_conf:
        common:
          user: "graphite"
          max-cpu: 2
        whisper:
          data-dir: "/var/local/whisper"
        cache:
          max-size: 2500000
      go_carbon_storage_schemas:
        default:
          pattern: ".*"
          retentions: "60s:30d,1h:5y"
      go_carbon_storage_aggregation_rules:
        default:
          pattern: ".*"
          xFilesFactor: 0.5
          aggregationMethod: "average"
    }
  }
```

License
-------

GPLv3

Author Information
------------------

This role was originally created by [Davide Ferrari](https://github.com/vide) while working for [Stuart](https://stuart.com/). If you like what we do, give me a shout! [We are hiring!](https://stuart.com/jobs/)
