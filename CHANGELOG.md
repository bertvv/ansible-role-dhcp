# Change log

This file contains al notable changes to the dhcp Ansible role.

This file adheres to the guidelines of [http://keepachangelog.com/](http://keepachangelog.com/). Versioning follows [Semantic Versioning](http://semver.org/).

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

