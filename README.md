# apache2 Cookbook

[![Cookbook Version](https://img.shields.io/cookbook/v/apache2.svg)](https://supermarket.chef.io/cookbooks/apache2)
[![Build Status](https://img.shields.io/circleci/project/github/sous-chefs/apache2/master.svg)](https://circleci.com/gh/sous-chefs/apache2)
[![OpenCollective](https://opencollective.com/sous-chefs/backers/badge.svg)](#backers)
[![OpenCollective](https://opencollective.com/sous-chefs/sponsors/badge.svg)](#sponsors)
[![pullreminders](https://pullreminders.com/badge.svg)](https://pullreminders.com?ref=badge)

This cookbook provides a complete Debian/Ubuntu style Apache HTTPD configuration. Non-Debian based distributions such as Red Hat/CentOS, ArchLinux and others supported by this cookbook will have a configuration that mimics Debian/Ubuntu style as it is easier to manage with Chef.

Debian-style Apache configuration uses scripts to manage modules and sites (vhosts). The scripts are:

- a2ensite
- a2dissite
- a2enmod
- a2dismod
- a2enconf
- a2disconf

This cookbook ships with templates of these scripts for non-Debian based platforms.

## Cookbooks

Depending on your OS configuration and security policy, you may need additional recipes or cookbooks for this cookbook's recipes to converge on the node. In particular, the following Operating System settings may affect the behavior of this cookbook:

- SELinux enabled
- Firewalls (such as iptables, ufw, etc.)
- Compile tools
- 3rd party repositories

On RHEL, SELinux is enabled by default. The [selinux](https://supermarket.chef.io/cookbooks/selinux) cookbook contains a `permissive` recipe that can be used to set SELinux to "Permissive" state. Otherwise, additional recipes need to be created by the user to address SELinux permissions.

To deal with firewalls Chef Software does provide an [iptables](https://supermarket.chef.io/cookbooks/iptables) and [ufw](https://supermarket.chef.io/cookbooks/ufw) cookbook but is migrating from the approach used there to a more robust solution utilizing the general [firewall](https://supermarket.chef.io/cookbooks/firewall) cookbook to setup rules. See those cookbooks' READMEs for documentation.

On ArchLinux, if you are using the `apache2::mod_auth_openid` recipe, you also need the [pacman](https://supermarket.chef.io/cookbooks/pacman) cookbook for the `pacman_aur` LWRP. Put `recipe[pacman]` on the node's expanded run list (on the node or in a role). This is not an explicit dependency because it is only required for this single recipe and platform; the pacman default recipe performs `pacman -Sy` to keep pacman's package cache updated.

## Platforms

The following platforms and versions are tested and supported using [test-kitchen](http://kitchen.ci/)

- Ubuntu 16.04 / 18.04
- Debian 8/9
- CentOS 7+
- Fedora Latest
- OpenSUSE Leap

### Notes for RHEL Family

Apache2.4 support for Centos 6 is not officially supported.

## Usage

It is recommended to create a project or organization specific [wrapper cookbook](https://www.chef.io/blog/2013/12/03/doing-wrapper-cookbooks-right/) and add the desired custom resources to the run list of a node. Depending on your environment, you may have multiple roles that use different recipes from this cookbook. Adjust any attributes as desired.

```ruby
# service['apache2'] is defined in the apache2_default_install resource but other resources are currently unable to reference it.  To work around this issue, define the following helper in your cookbook:
service 'apache2' do
  extend Apache2::Cookbook::Helpers
  service_name lazy { apache_platform_service_name }
  supports restart: true, status: true, reload: true
  action :nothing
end

apache2_install 'default_install'
apache2_module 'headers'
apache2_module 'ssl'

apache2_default_site 'foo' do
  default_site_name 'my_site'
  cookbook 'my_cookbook'
  port '443'
  template_source 'my_site.conf.erb'
  action :enable
end
```

Example wrapper cookbooks:
[basic site](https://github.com/sous-chefs/apache2/blob/master/test/cookbooks/test/recipes/basic_site.rb)
[ssl site](https://github.com/sous-chefs/apache2/blob/master/test/cookbooks/test/recipes/mod_ssl.rb)

## Resources

- [install](documentation/resource_apache2_install.md)
- [default_site](documentation/resource_apache2_default_site.md)
- [site](documentation/resource_apache2_site.md)
- [conf](documentation/resource_apache2_conf.md)
- [config](documentation/resource_apache2_config.md)
- [mod](documentation/resource_apache2_mod.md)
- [module](documentation/resource_apache2_module.md)

## Contributors

This project exists thanks to all the people who [contribute.](https://opencollective.com/sous-chefs/contributors.svg?width=890&button=false)

### Backers

Thank you to all our backers! 🙏 [Become a backer](https://opencollective.com/sous-chefs#backer)
![https://opencollective.com/sous-chefs#backers](https://opencollective.com/sous-chefs/backers.svg?width=890)

### Sponsors

Support this project by becoming a sponsor. Your logo will show up here with a link to your website. [Become a sponsor](https://opencollective.com/sous-chefs#sponsor)
![https://opencollective.com/sous-chefs/sponsor/0/website](https://opencollective.com/sous-chefs/sponsor/0/avatar.svg?sanitize=true)
![https://opencollective.com/sous-chefs/sponsor/1/website](https://opencollective.com/sous-chefs/sponsor/1/avatar.svg)
![https://opencollective.com/sous-chefs/sponsor/2/website](https://opencollective.com/sous-chefs/sponsor/2/avatar.svg)
![https://opencollective.com/sous-chefs/sponsor/3/website](https://opencollective.com/sous-chefs/sponsor/3/avatar.svg)
![https://opencollective.com/sous-chefs/sponsor/4/website](https://opencollective.com/sous-chefs/sponsor/4/avatar.svg)
![https://opencollective.com/sous-chefs/sponsor/5/website](https://opencollective.com/sous-chefs/sponsor/5/avatar.svg)
![https://opencollective.com/sous-chefs/sponsor/6/website](https://opencollective.com/sous-chefs/sponsor/6/avatar.svg)
![https://opencollective.com/sous-chefs/sponsor/7/website](https://opencollective.com/sous-chefs/sponsor/7/avatar.svg)
![https://opencollective.com/sous-chefs/sponsor/8/website](https://opencollective.com/sous-chefs/sponsor/8/avatar.svg)
![https://opencollective.com/sous-chefs/sponsor/9/website](https://opencollective.com/sous-chefs/sponsor/9/avatar.svg)

## License

```text
Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```
