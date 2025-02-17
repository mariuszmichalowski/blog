---
title: "Vagrant & Puppet"
author: "Russ Mckendrick"
date: 2014-01-12T12:00:00.000Z
lastmod: 2021-07-31T12:31:08+01:00
tags:
 - Tech
 - Centos
 - DevOps
 - Puppet
 - Vagrant
cover:
    image: "/img/2014-01-12_vagrant-puppet_0.png" 
images:
 - "/img/2014-01-12_vagrant-puppet_0.png"

aliases:
- "/vagrant-puppet-237b2b68dec"

---
Vagrant allows developers to bring up local development machines in a controlled method using pre-built configuration files called boxes. Once a box has been installed on the developers local machine it can be powered on with the following command;

```
vagrant up
```

When the box is brought up it automatically provisions itself using a number of provisioners including Puppet, this means that the production deployment scripts can also be run on local development copies of the instances. The instances on the local machines are powered by Virtualbox and there Windows, Linux and Mac versions of Vagrant.

Once a local instance has booted it can attach a folder on the developers local machine to the virtual server, this means that a developer can work on a local copy of the code within their chosen IDE and be able to test the code instantly on an environment which matches the production environment as closely as possible, all without the need to deploy multiple servers within a hosting environment.

A typical Vagrant session looks like the following;

```
russ$ vagrant up
Bringing machine ‘default’ up with ‘virtualbox’ provider…
[default] Importing base box ‘CentOS-Dev-6.5’…
[default] Matching MAC address for NAT networking…
[default] Setting the name of the VM…
[default] Clearing any previously set forwarded ports…
[default] Creating shared folders metadata…
[default] Clearing any previously set network interfaces…
[default] Preparing network interfaces based on configuration…
[default] Forwarding ports…
[default] — 22 => 2222 (adapter 1)
[default] — 80 => 8888 (adapter 1)
[default] Booting VM…
[default] Waiting for machine to boot. This may take a few minutes…
[default] Machine booted and ready!
[default] Setting hostname…
[default] Configuring and enabling network interfaces…
[default] Mounting shared folders…
[default] — /data
[default] — /vagrant
[default] — /webroot
[default] — /tmp/vagrant-puppet/manifests
[default] — /tmp/vagrant-puppet/modules-0
[default] Running provisioner: puppet…
Running Puppet with vagrant.pp…
Warning: Config file /etc/puppet/hiera.yaml not found, using Hiera defaults
Notice: Compiled catalog for dev.local in environment production in 2.30 seconds
Notice: /Stage[main]/Common::Apps/Package[tree]/ensure: created
Notice: /Stage[main]/Mysql::Client::Install/Package[mysql_client]/ensure: created
Notice: /Stage[main]/Common::Repos/Package[epel-release-6–8.noarch]/ensure: created
Notice: /Stage[main]/Common::Apps/Package[man]/ensure: created
Notice: /Stage[main]/Common::Apps/Package[dstat]/ensure: created
Notice: /Stage[main]/Common::Apps/Package[mailx]/ensure: created
Notice: /Stage[main]/Common::Apps/Package[lsof]/ensure: created
Notice: /Stage[main]/Services::Apache/Package[httpd-devel]/ensure: created
Notice: /Stage[main]/Mysql::Server::Install/Package[mysql-server]/ensure: created
Notice: /Stage[main]/Mysql::Server::Config/File[/etc/mysql]/ensure: created
Notice: /Stage[main]/Mysql::Server::Config/File[/etc/my.cnf]/content: content changed ‘{md5}8ace886bbe7e274448bc8bea16d3ead6’ to ‘{md5}7bbc1d3914d13dd7960d5fa5bc5a80c6’
Notice: /Stage[main]/Ntp::Install/Package[ntp]/ensure: created
Notice: /Stage[main]/Ntp::Config/File[/etc/ntp.conf]/content: content changed ‘{md5}7fda24f62b1c7ae951db0f746dc6e0cc’ to ‘{md5}2396e8417e644318c99399b0460d93eb’
Notice: /Stage[main]/Services::Phpmyadmin/Package[phpMyAdmin]/ensure: created
Notice: /Stage[main]/Services::Phpmyadmin/File[/etc/phpMyAdmin/config.inc.php]/content: content changed ‘{md5}cc6db4fb4e08b40c637fa096e55f4bd5’ to ‘{md5}ad2fd178c0bbd0061196a3a3bc24af19’
Notice: /Stage[main]/Ntp::Service/Service[ntp]/ensure: ensure changed ‘stopped’ to ‘running’
Notice: /Stage[main]/Services::Apache/File[/etc/httpd/conf.d/vhost.conf]/ensure: created
Notice: /Stage[main]/Services::Php/Package[php-pecl-apc]/ensure: created
Notice: /Stage[main]/Services::Php/Package[php-devel]/ensure: created
Notice: /Stage[main]/Services::Php/Package[php-soap]/ensure: created
Notice: /Stage[main]/Services::Php/Package[php-ldap]/ensure: created
Notice: /Stage[main]/Git::Install/Package[git]/ensure: created
Notice: /Stage[main]/Git::Install/Package[git-svn]/ensure: created
Notice: /Stage[main]/Mysql::Server::Config/File[/etc/mysql/conf.d]/ensure: created
Notice: /Stage[main]/Mysql::Server::Service/Service[mysqld]/ensure: ensure changed ‘stopped’ to ‘running’
Notice: /Stage[main]/Mysql::Server::Root_password/Mysql_user[root@localhost]/password_hash: defined ‘password_hash’ as ‘*EA791BBC44F8413FFF8C3E939FE24752D4E84FC7’
Notice: /Stage[main]/Mysql::Server::Root_password/File[/root/.my.cnf]/ensure: defined content as ‘{md5}67b81309e1920b5a39c836c83b455316’
Notice: /Stage[main]/Services::Mysqldatabase/Mysql::Db[vagrant]/Mysql_database[vagrant]/ensure: created
Notice: /Stage[main]/Services::Mysqldatabase/Mysql::Db[vagrant]/Mysql_user[vagrant@localhost]/ensure: created
Notice: /Stage[main]/Services::Mysqldatabase/Mysql::Db[vagrant]/Mysql_grant[vagrant@localhost/vagrant.*]/ensure: created
Notice: /Stage[main]/Services::Phpmyadmin/File[/etc/httpd/conf.d/phpMyAdmin.conf]/content: content changed ‘{md5}b6653d976dbab702b2bec270adc4b6f4’ to ‘{md5}d0c590e1018012bd33cd9fa74f5d28bc’
Notice: /Stage[main]/Services::Apache/Service[httpd]/ensure: ensure changed ‘stopped’ to ‘running’
Notice: /Stage[main]/Services::Apache/File[/webroot/public_html]/ensure: created
Notice: /Stage[main]/Services::Git/Git::Repo[vagrant-dev-box]/Exec[git_repo_vagrant-dev-box]/returns: executed successfully
Notice: /Stage[main]/Services::Php/Package[php-xml]/ensure: created
Notice: Finished catalog run in 260.98 seconds
```

This brings up a CentOS server, port maps port 8888 on the local machine to port 80 on the instance and then mounts ~/Documents/Vagrant/apache/webroot/ as /webroot on the instance. Once the instance has booted it then executes a Puppet manifest which installs & configures the following services;

- Apache
- PHP
- MySQL
- PHPMyAdmin
- Also as other tools such as VIM etc are installed

Once installed it checks out code from a Github repository into /webroot ready for coding to start, the code is also available in the local browser at [http://localhost:8888/.](http://localhost:8888/.)

Once the instance has been finished with you can remove it or power it down ready to use again, neither of these actions will remove the code or GIT configuration from the local copy in the `~/Documents/Vagrant/apache/webroot/` directory.

Vagrant is an Open Source project released under the MIT license and can be [found here](http://www.vagrantup.com/)

For my own [Vagrant configuration check my GitHub repo](https://github.com/russmckendrick/vagrant-puppet)