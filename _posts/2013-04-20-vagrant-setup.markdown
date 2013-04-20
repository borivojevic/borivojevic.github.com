---
layout: post
title: Blog
subtitle: Setup Vagrant Instance
category: posts
---

````php
    if (Configure::read('debug') > 0) {
        CakePlugin::load('CodingStandards', array('bootstrap' => true));
        //Configure::write('CodingStandards.ADDITIONAL_PATHS', array('CodingStandards' => Configure::read('CodingStandards.PLUGIN_PATH'))); // Optional - Useful if you have extra paths you want included in full reports.  Example here is the coding standards themselves, though you can other other(s).
        //Configure::write('CodingStandards.SERVER_NAME', '<Insert Accessible URL HERE>') // Optional and probably server specific -- enables CSS checking & provides full URL for HTML reports
        //Also see See app/Plugin/CodingStandards/Config/bootstrap.php for other variables you can tweak
    }
````

Requirements:
---------------
- Download and install [Vagrant][]
- Vagrant depends on Oracle's [VirtualBox][] to create all of its virtual environments.

Installation:
---------------
- Download and install required software
- `git clone git://github.com/borivojevic/vagrant-setup.git`
- Place application source code into webroot folder
- Add `127.0.0.1 dev.mirkoborivojevic.localhost` to hosts file
- Run terminal and execute `vagrant up`
- Run web browser and go to `dev.mirkoborivojevic.localhost:8888`
- To log in to vagrant box execute `ssh vagrant@127.0.0.1 -p 2222`
- To turn off virtual machine execute `vagrant down`
- To clean up execute `vagrant destroy`

Packages installed
-------------------
- apache2
- php5
- php5-cli
- php5-mysql
- php5-dev
- php-pear
 - PHPUnit
 - PHP_CodeSniffer
 - CakePHP_CodeSniffer
- mysql-server
- git-core
- vim
- curl
- composer


Nice to have (not installed yet)
--------------------------------
- phpmyadmin
- cakephp
- xdebug

[Vagrant]: http://downloads.vagrantup.com/tags/v1.0.3
[VirtualBox]: http://www.virtualbox.org/wiki/Downloads

Troubleshooting
---------------

Vagrant sometimes hangs on "Waiting for VM to boot. This can take a few minutes". To fix this enable GUI mode in Vagrant configuration, login in VirtualBox and run "sudo dhclient".