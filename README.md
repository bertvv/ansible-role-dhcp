# Ansible role `dhcp`

Ansible role for setting up ISC DHCPD on RHEL/CentOS 7. Specifically, the responsibilities of this role are to:

- Install packages
- Manage configuration ([dhcpd.conf(5)](http://linux.die.net/man/5/dhcpd.conf))

The following are NOT concerns of this role:

- Managing firewall configuration: Use e.g. [bertvv.el7](https://galaxy.ansible.com/list#/roles/2305) for this.

## Requirements

No specific requirements

## Role Variables

This role is able to set (some) global options, and to specify subnet declarations.

See the [test playbook](tests/test.yml) for a working example of a DHCP server with two subnets. This section is mainly a reference of all supported options.

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

### Subnet declarations

The role variable `dhcp_subnets` contains a list of dicts, specifying the subnet declarations to be added to the DHCP configuration file. Every subnet declaration should have an `ip` and `netmask`, other options are not mandatory. We start this section with an example, a compelete overview of supported options follows.

```Yaml
dhcp_subnets:
  - ip: 192.168.222.0
    netmask: 255.255.255.128
    domain_name_servers:
      - 10.0.2.3
      - 10.0.2.4
    range_begin: 192.168.222.50
    range_end: 192.168.222.127
  - ip: 192.168.222.128
    default_lease_time: 3600
    max_lease_time: 7200
    netmask: 255.255.255.128
    domain_name_servers: 10.0.2.3
    routers: 192.168.222.129
    hosts:
      - name: cl1
        mac: '00:11:22:33:44:55'
        ip: 192.168.222.150
      - name: cl2
        mac: '00:de:ad:be:ef:00'
        ip: 192.168.222.151
```

An alphabetical list of supported options in a subnet declaration:

| Option                | Required | Comment                                                               |
| :---                  | :---:    | :--                                                                   |
| `default_lease_time`  | no       | Default lease time for this subnet (in seconds)                       |
| `domain_name_servers` | no       | List of domain name servers for this subnet(1)                        |
| `domain_search`       | no       | List of domain names for resolution of non-FQDNs(1)                   |
| `hosts`               | no       | A list of dicts specifying host declarations. See below.              |
| `ip`                  | yes      | **Required.** IP address of the subnet                                |
| `max_lease_time`      | no       | Maximum lease time for this subnet (in seconds)                       |
| `netmask`             | yes      | **Required.** Network mask of the subnet (in dotted decimal notation) |
| `range_begin`         | no       | Lowest address in the range of dynamic IP addresses to be assigned    |
| `range_end`           | no       | Highest address in the range of dynamic IP addresses to be assigned   |
| `routers`             | no       | IP address of the gateway for this subnet                             |
| `subnet_mask`         | no       | Overrides the `netmask` of the subnet declaration                     |

You can specify hosts that should get a fixed IP address based on their MAC by setting the `hosts` option. This is a list of dicts with the following three keys, all of which are mandatory:

| Option | Comment                                   |
| :---   | :---                                      |
| `name` | The name of the host                      |
| `mac`  | The MAC address of the host               |
| `ip`   | The IP address to be assigned to the host |

## Dependencies

No dependencies.

## Example Playbook

See the [test playbook](tests/test.yml)

## Testing

The `tests` directory contains tests for this role in the form of a Vagrant environment.  The playbook [`test.yml`](tests/test.yml) sets up a DHCP server with some global options set and two subnet declarations.

The directory `tests/roles/dhcp` is a symbolic link that should point to the root of this project in order to work. If you need to (re)create it, do

```ShellSession
$ cd tests/
$ mkdir roles
$ ln -frs ../../PROJECT_DIR roles/dhcp
```

You may want to change the base box into one that you like. The current one is based on Box-Cutter's [CentOS Packer template](https://github.com/boxcutter/centos).


## Contributing

Issues, feature requests, ideas are appreciated and can be posted in the Issues section. Pull requests are also very welcome. Preferably, create a topic branch and when submitting, squash your commits into one (with a descriptive message).

## License

BSD

## Author Information

Bert Van Vreckem (bert.vanvreckem@gmail.com)

