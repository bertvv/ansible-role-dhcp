# Change log

This file contains all notable changes to the dhcp Ansible role.

This file adheres to the guidelines of [http://keepachangelog.com/](http://keepachangelog.com/). Versioning follows [Semantic Versioning](http://semver.org/).

## 3.1.0 - 2021-11-30

### Added

- (GH-44) Add support for RHEL 8, and its derivatives. (credit: [Stuart Knight](https://github.com/blofeldthefish))
- (GH-43) Allow to specify multiple ranges in a subnet declaration (credit: [Fabio Rocha](https://github.com/frock81))
- (GH-49) Add option to distribute hostnames through DHCP (credit: [Jan Bundesmann](https://github.com/knoppi))
- (GH-52) Add support for FreeBSD (credit: [Josh Cummings](https://github.com/jrac))
- (GH-60) Allow setting the network interface for subnet declarations (credit: [Valentin Delaye](https://github.com/jonesbusy))

### Changed

- (GH-45) If a custom config file has .j2 as extension, remove it when installing on the target system (credit: [Fabio Rocha](https://github.com/frock81))
- (GH-46) Allow setting permissions on custom config files (credit: [Fabio Rocha](https://github.com/frock81))

## 3.0.2 - 2019-08-29

### Added

- (GH-29) The ability to add customised config snippets, whilst using a locally defined (outside of this role) Jinja Template.  (credit: [Alex Gittings](https://github.com/minitriga))

## 3.0.1 - 2019-08-14

### Changed

- Fix ansible-lint warnings
- Update documentation for failover peer documentation

## 3.0.0 - 2019-08-14

### Added

- (GH-18) The ability to add multiple subnet ranges to a scope. (credit: [Stuart Knight](https://github.com/blofeldthefish))
- (GH-24) Add parameter `dhcp_apparmor_fix` to enable/disable the AppArmor fix (credit: [Maxim Baranov](https://github.com/mbaran0v))
- Variable `dhcp_pxeboot_server` in order to allow this role to refer PXEBoot clients to the correct PXEBoot server.

### Changed

- (GH-19) **Breaking change** Fix inconsistency with readme for omapi secret. In the README the `dhcp_global_omapi_secret` is defined as such, whereas in the template it is `dhcp_omapi_secret`. It *should* be `dhcp_global_omapi_secret`. This will break playbooks that use the `dhcp_omapi_secret` variable.
- (GH-21, GH-25) Define network device in /etc/defaults. This is needed on Debian based distros.
- (GH-22) Support `include` lines for non-existent files in role's `files/` directory. This allows the user to add `include` lines in dhcpd.conf for non-existent files; files not found in role's `files/` directory. It should permit successful configuration of `dhcpd.conf` with the expectation of another process (role, task, legacy method, etc.) to provide the include file. (credit: [RayfordJ](https://github.com/rayfordj))
- (GH-23) Removed default value for `dhcp_global_other_options` and test for its definition in the config file template. This is more consistent with how the other role variables are handled in the config file template. (credit: [lijok](https://github.com/lijok))
- (GH-26) Fixed typo in README (credit [Guillaume Parent](https://github.com/gparent))
- (GH-27) Use list of packages directly instead of in a `with_items` loop. (credit [Guillaume Parent](https://github.com/gparent))
- Increased minimum Ansible version to 2.8 due to usage of more recent Ansible syntax (e.g. package installation directly with variable containing list of packages instead of `with_items` loop).
- Updated list of supported versions to latest stable releases of tested distros (EL 7.6, Fedora 30, Ubuntu 18.04)
- Use Yamllint configuration from Ansible Galaxy and fix Yamllint warnings
- Updated Vagrant test environment, in new orphan branch `vagrant-tests`.

## 2.2.0 - 2018-10-13

### Added

- (GH-13,14) support fixed address hosts in subnets (credit: [Ahmed Sghaier](https://github.com/asghaier))
- (GH-15) Add variable `dhcp_service_state`, to define the desired state of the service (default: started). (credit: [Alessandro Ogier](https://github.com/aogier))
- (GH-17) New configuration items for failover peer: `address`, `failover_peer`, `hba`, `load_balance_max_seconds`, `max-balance`, `max-lease-misbalance`, `max-lease-ownership`, `max_response_delay`, `max_unacked_updates`, `mclt`, `min-balance`, `peer_address`, `peer_port`, `port`, `role`, `split` (credit: [cacheira](https://github.com/cacheira))

### Changed

- (GH-11,12) The `domain_search` key of `dhcp_subnets` can now also be a list (credit: [Ahmed Sghaier](https://github.com/asghaier))
- (GH-16) Allow host declaration without specifying `fixed-address`. (credit: [Alessandro Ogier](https://github.com/aogier))

## 2.1.2 - 2017-11-21

### Changed

- Fixed Ansible 2.4 deprecation warnings (include: -> include_tasks)

## 2.1.1 - 2017-07-03

### Changed

- (GH-10) Fixed bug where playbook run fails because `dhcp_global_includes` is undefined

## 2.1.0 - 2017-06-26

### Added

- New configuration items:
    - (GH-7) `dhcp_global_log_facility`, `dhcp_global_server_name`, `dhcp_global_authoritative` (credit: [@jpiron](https://github.com/jpiron))
    - `dhcp_global_ntp_servers`, `dhcp_global_includes` (credit: Felix Egli)

- (GH-9) Support OMAPI keys and catch-all options (credit: [@joshbenner](https://github.com/joshbenner))

### Changed

- (GH-7) Several improvements: package state as variable instead of hard-coded, made host declarations global (credit: [@jpiron](https://github.com/jpiron)
- (GH-8) Fixed typo in README (credit: [@donvipre](https://github.com/donvipre)
- Quoted values in `dhcp_global_domain_search` (credit: Felix Egli)

## 2.0.0 - 2016-04-29

### Added

- Support for Ubuntu LTS 14.04 (Trusty Tahr) and 16.04 (Xenial Xerus)
- Tested on Fedora 23 and CentOS 6, and added to supported platforms

### Changed

- This version now uses the general package management module introduced in Ansible 2.0. This is considered a breaking change, since it wil no longer work with Ansible 1.6-1.7.

## 1.1.0 - 2016-04-28

### Added

- Support for PXE boot parameters bootp, booting, next-server, filename. Credits to [Rian Bogle](https://github.com/rbogle)
- Address pools within subnet declaration. Credits to [Birgit Croux](https://github.com/birgitcroux)
- Definition of classes with match statements

## 1.0.1 - 2015-08-28

### Changed

- Fixed a tag name
- Fixed GH-1: domain name no longer needs to be "double quoted"

## 1.0.0 - 2015-08-24

First release!

### Added

- Allow setting some global variables
- Subnet declarations in YAML
