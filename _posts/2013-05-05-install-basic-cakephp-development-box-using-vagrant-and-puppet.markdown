---
layout: post
title: Blog
subtitle: Install basic CakePHP development box using Vagrant and Puppet
category: posts
---

## Overview

Vagrant is a great tool to automate creating and configuring lightweight, reproducible, and portable development environments. If you are new to Vagrant you might want to take a look at [official documentation](http://docs.vagrantup.com/v2/why-vagrant/index.html) first to get a basic gist who is it for and why to use it.

## Host OS software prerequisites

- Vagrant depends on Oracle's [VirtualBox][]. Download and install it first.
- Download and install [Vagrant][]

## Installation procedure

1. Make sure you've installed prerequisites
2. Open terminal, <code>cd</code> to working directory and clone the project:
    <pre class="terminal"><code>git clone git://github.com/borivojevic/cakephp-vagrant.git</code></pre>
3. Place application source code into cakephp-vagrant/webroot folder
4. On the host machine, add a new line to your <code>hosts</code> file:
    <code>33.33.33.10 dev.mirkoborivojevic.localhost</code>

## Day to day usage

- Open terminal and <code>cd</code> to working directory
- Run <code>vagrant up</code> turn on virtual machine machine
- Open web browser and point it to <code>dev.mirkoborivojevic.localhost:8888</code>
- Log in to vagrant box with <code>ssh vagrant@127.0.0.1 -p 2222</code>
- Turn off virtual machine with <code>vagrant down</code>
- Turn off and destroy virtual machine with <code>vagrant destroy</code>

## Default connection parameters

    Virtual machine IP: 33.33.33.10
    System user: vagrant
    System password: vagrant
    MySQL user: root
    MySQL password: root
    Apache Virtual Host: dev.mirkoborivojevic.localhost

## Packages and libraries that come with the box

- apache2
- php5 (5.3)
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

## Troubleshooting

VirtualBox sometimes hangs on "Waiting for VM to boot. This can take a few minutes". To fix this enable GUI mode in Vagrant configuration, login in VirtualBox and run "sudo dhclient".

## TODO

- Additional libraries
    - phpmyadmin
    - cakephp
    - xdebug
- Add PHP 5.4 support
- Support multiple projects and mountpoints in Vagrantfile (see http://goo.gl/TDACB)

[Vagrant]: http://downloads.vagrantup.com/tags/v1.0.3
[VirtualBox]: http://www.virtualbox.org/wiki/Downloads