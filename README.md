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
hierarchy:
- name: Yaml backend
  data_hash: yaml_data
  paths:
  - "nodes/%{::trusted.certname}"
  - 'common'
  datadir: "/etc/puppetlabs/code/environments/%{environment}/hieradata"
```

Convert a test hiera.yaml file

```
[root@master ~]# rake convert[/path/to/other.yaml]
hierarchy:
- name: Yaml backend
  data_hash: yaml_data
  paths:
  - "nodes/%{::trusted.certname}"
  - 'common'
  datadir: "/etc/puppetlabs/code/environments/%{environment}/hieradata"
- name: Eyaml backend
  lookup_key: eyaml_lookup_key
  paths:
  - "nodes/%{::trusted.certname}"
  - 'common'
  datadir: "/etc/puppetlabs/code/environments/%{environment}/hieradata"
  options:
    pkcs7_private_key: "/path/to/private_key.pkcs7.pem"
    pkcs7_public_key: "/path/to/public_key.pkcs7.pem"
```

## Notes

Currently, it is assumed that only `yaml` and `json` backends use `data_hash`
and only the `eyaml` backend uses `lookup_key`.  In order to add more backends
to these categories, you can do the following:

```
conversion = HieraFiver.new(config_file)
conversion.backends_data_hash  = (['yaml', 'json', 'someotherhash'])
conversion.backends_lookup_key = (['eyaml', 'someotherlookup'])
```

For more information, read about [Three kinds of backends](https://docs.puppet.com/puppet/latest/hiera_custom_backends.html#three-kinds-of-backends).
