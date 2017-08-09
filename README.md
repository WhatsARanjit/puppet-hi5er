# Hi5er

#### Table of Contents

1. [Overview](#overview)
1. [Requirements](#requirements)

## Overview

Convert a Hiera 3 file into Hiera 5 format

* Puppet >= 3.7.1

## Usage

Convert currently configured hiera_config file

```
[root@master ~]# rake
---
version: 5
hierarchy:
- name: Yaml backend
  data_hash: yaml_data
  paths:
  - "nodes/%{::trusted.certname}.yaml"
  - 'common.yaml'
  datadir: "/etc/puppetlabs/code/environments/%{environment}/hieradata"
```

Convert a test hiera.yaml file

```
[root@master ~]# rake convert[/path/to/other.yaml]
---
version: 5
hierarchy:
- name: Yaml backend
  data_hash: yaml_data
  paths:
  - "nodes/%{::trusted.certname}.yaml"
  - 'common.yaml'
  datadir: "/etc/puppetlabs/code/environments/%{environment}/hieradata"
- name: Eyaml backend
  lookup_key: eyaml_lookup_key
  paths:
  - "nodes/%{::trusted.certname}.eyaml"
  - 'common.eyaml'
  datadir: "/etc/puppetlabs/code/environments/%{environment}/hieradata"
  options:
    pkcs7_private_key: "/path/to/private_key.pkcs7.pem"
    pkcs7_public_key: "/path/to/public_key.pkcs7.pem"
```

## Notes

Currently, it is assumed that only `yaml`, `json`, and `hocon`  backends use 
`data_hash` and only the `eyaml` backend uses `lookup_key`.  Backends for 
`data_dig` are currently empty.  In order to add more backends to these 
categories, you can add the following to the rake task:

```
conversion = HieraFiver.new(config_file)
conversion.backends_data_hash  = (['yaml', 'json', 'hocon', 'someotherhash'])
conversion.backends_lookup_key = (['eyaml', 'someotherlookup'])
conversion.backends_data_dig   = (['somedatadig'])
```

For more information, read about [Three kinds of backends](https://docs.puppet.com/puppet/latest/hiera_custom_backends.html#three-kinds-of-backends).
