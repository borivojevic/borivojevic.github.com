---
layout: post
title: Blog
subtitle: Install basic CakePHP development box using Vagrant and Puppet
category: posts
---

## Overview

Vagrant is a great tool to automate creating and configuring lightweight, reproducible, and portable development environments. If you are new to Vagrant you might want to take a look at [official documentation](http://docs.vagrantup.com/v2/why-vagrant/index.html) to get a basic gist who is it for and why to use it.

## Host OS software prerequisites:

- Vagrant depends on Oracle's [VirtualBox][] to create all of its virtual environments.
- Download and install [Vagrant][]

## Installation:

- Make sure you've installed prerequisites
- Open terminal, {% highlight bash %}cd{% endhighlight %} to working directory and clone the project
    {% highlight bash %} git clone git://github.com/borivojevic/cakephp-vagrant.git {% endhighlight %}
- Place application source code into cakephp-vagrant/webroot folder
- On the host machine, add a new line to your {% highlight bash %}hosts{% endhighlight %} file pointing to vagrant box {% highlight bash %}33.33.33.10 dev.mirkoborivojevic.localhost{% endhighlight %}
- Run {% highlight bash %}vagrant up{% endhighlight %} to provision machine
- Run web browser and go to {% highlight bash %}dev.mirkoborivojevic.localhost:8888{% endhighlight %}
- To log in to vagrant box execute {% highlight bash %}ssh vagrant@127.0.0.1 -p 2222{% endhighlight %}
- To turn off virtual machine execute {% highlight bash %}vagrant down{% endhighlight %}
- To clean up execute {% highlight bash %}vagrant destroy{% endhighlight %}

## Default connection parameters

    Virtual machine IP:     33.33.33.10
    System user:            vagrant
    System password:        vagrant
    MySQL user:             root
    MySQL password:         root
    Apache Virtual Host:    dev.mirkoborivojevic.localhost

## Packages and libraries that come with the box

- apache2
- php5 (5.3)
- php5-cli
- php5-mysql
- php5-dev
- php-pear
    PHPUnit
    PHP_CodeSniffer
    CakePHP_CodeSniffer
- mysql-server
- git-core
- vim
- curl
- composer

## TODO

- Additional libraries
    phpmyadmin
    cakephp
    xdebug
- Add PHP 5.4 support
- Support multiple projects and mountpoints in Vagrantfile (see http://goo.gl/TDACB)

[Vagrant]: http://downloads.vagrantup.com/tags/v1.0.3
[VirtualBox]: http://www.virtualbox.org/wiki/Downloads

## Troubleshooting

VirtualBox sometimes hangs on "Waiting for VM to boot. This can take a few minutes". To fix this enable GUI mode in Vagrant configuration, login in VirtualBox and run "sudo dhclient".