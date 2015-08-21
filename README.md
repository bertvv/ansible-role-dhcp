# Ansible role `dhcp`

Ansible role for setting up ISC DHCPD on RHEL/CentOS 7. Specifically, the responsibilities of this role are to:

- Install packages
- Manage configuration

The following are NOT concerns of this role:

- Managing firewall configuration: Use e.g. [bertvv.el7](https://galaxy.ansible.com/list#/roles/2305) for this.

## Requirements

No specific requirements

## Role Variables

This role is able to set (some) global options, and to declare subnets.

### Global options

The following variables, when set, will be added to the global section of the DHCP configuration file. There is no default value, so when they are not set, they will be left out.

See the [dhcp-options(5)](http://linux.die.net/man/5/dhcp-options) man page for more information about these options.

| Variable                          | Comments                                                                |
| :---                              | :---                                                                    |
| `dhcp_global_broadcast_address`   | Global broadcast address                                                |
| `dhcp_global_default_lease_time`  | Default lease time in seconds                                           |
| `dhcp_global_domain_name`         | The domain name the client should use when resolving host names         |
| `dhcp_global_domain_name_servers` | A list of IP addresses of DNS servers(1)                                |
| `dhcp_global_domain_search`       | A list of domain names too be used by the client to locate non-FQDNs(1) |
| `dhcp_global_max_lease_time`      | Maximum lease time in seconds                                           |
| `dhcp_global_routers`             | IP address of the router                                                |
| `dhcp_global_subnet_mask`         | Global subnet mask                                                      |

(1) This option may be written either as a list (when you have more than one item), or as a string (when you have only one). The following snippet shows an example of both:

```Yaml
# A single DNS servr
dhcp_global_domain_name_servers: 8.8.8.8

# A list of DNS servers
dhcp_global_domain_name_servers:
  - 8.8.8.8
  - 8.8.4.4
```

## Dependencies

No dependencies.

## Example Playbook

See the [test playbook](tests/test.yml)

## Testing

The `tests` directory contains tests for this role in the form of a Vagrant environment. The directory `tests/roles/dhcp` is a symbolic link that should point to the root of this project in order to work. To create it, do

```ShellSession
$ cd tests/
$ mkdir roles
$ ln -frs ../../PROJECT_DIR roles/dhcp
```

You may want to change the base box into one that you like. The current one is based on Box-Cutter's [CentOS Packer template](https://github.com/boxcutter/centos).

The playbook [`test.yml`](tests/test.yml) applies the role to a VM, setting role variables.

## Contributing

Issues, feature requests, ideas are appreciated and can be posted in the Issues section. Pull requests are also very welcome. Preferably, create a topic branch and when submitting, squash your commits into one (with a descriptive message).

## License

BSD

## Author Information

Bert Van Vreckem (bert.vanvreckem@gmail.com)

