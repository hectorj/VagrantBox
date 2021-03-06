# LAMP Vagrant box

Sources to build our preconfigured **LAMP** + **Redis** + **RabbitMQ** + **Elasticsearch** + **Tools** Vagrant box.

## Usage

This box is available on [Hashicorp Atlas](https://atlas.hashicorp.com/kuikui/boxes/modern-lamp).

To use it, juste add these lines to your `Vagrantfile` :

```
config.vm.box         = 'kuikui/modern-lamp'
config.vm.box_version = '>=2.3.0'
```

## Dependencies

Install [Virtualbox](https://www.virtualbox.org/wiki/Downloads) and [packer](http://www.packer.io/downloads.html).

## Build

```
$ git clone git@github.com:KuiKui/VagrantBox.git
$ cd VagrantBox
$ packer build template.json
```

For debug purpose :
```
$ PACKER_LOG=1 packer build template.json
```

## Test

```
$ vagrant box add kuikui-test kuikui-modern-lamp.box --force
$ vagrant up
```

## Usage

Add the new built box to vagrant :
```
$ vagrant box add kuikui kuikui-modern-lamp.box
```

And use the `box` instruction in your Vagrantfile to select the box :
```ruby
config.vm.box = "kuikui"
```

Use the [test Vagrantfile](Vagrantfile) at the root of your project or create your own, and launch vagrant :
```
$ vagrant up
$ vagrant ssh
```

Then :

* source files are in `/vagrant`
* Apache root directory is `/vagrant/web`
* MySQL is up with user `root` and password `vagrant`
* Redis is up on `127.0.0.1:6379`
* RabbitMQ is up with user `guest` and password `guest`

And :

* http server can be reached from local env at `http://localhost:8080`
* RabbitMQ web UI can be reached from local env at `http://localhost:15672`
* Elasticsearch Head plugin can be reached from local env at `http://localhost:9200/_plugin/head/`

## Specs

**Linux**
```
$ cat /etc/debian_version
8.5
```
```
$ cat /etc/timezone
Europe/Paris
```

**Apache**
```
$ sudo apache2 -v
Server version: Apache/2.4.10 (Debian)
Server built:   Sep 15 2016 20:44:43
```

**MySQL**
```
$ sudo mysqld --version
mysqld  Ver 5.5.54-0+deb8u1 for debian-linux-gnu on x86_64 ((Debian))
```
```
$ mysql --version
mysql  Ver 14.14 Distrib 5.5.54, for debian-linux-gnu (x86_64) using readline 6.3
```

**PHP**
```
$ php -v
PHP 7.1.2-3+0~20170222101552.17+jessie~1.gbp444202 (cli) (built: Feb 22 2017 10:50:30) ( NTS )
Copyright (c) 1997-2017 The PHP Group
Zend Engine v3.1.0, Copyright (c) 1998-2017 Zend Technologies
    with Zend OPcache v7.1.2-3+0~20170222101552.17+jessie~1.gbp444202, Copyright (c) 1999-2017, by Zend Technologies
    with Xdebug v2.5.0, Copyright (c) 2002-2016, by Derick Rethans
```
```
$ php -m
[PHP Modules]
bcmath
calendar
Core
ctype
curl
date
dom
exif
fileinfo
filter
ftp
gd
geoip
gettext
gmp
hash
iconv
igbinary
intl
json
ldap
libxml
mbstring
mcrypt
memcached
msgpack
mysqli
mysqlnd
openssl
pcntl
pcre
PDO
pdo_mysql
pdo_sqlite
Phar
posix
readline
redis
Reflection
session
shmop
SimpleXML
sockets
SPL
sqlite3
standard
sysvmsg
sysvsem
sysvshm
tokenizer
wddx
xdebug
xml
xmlreader
xmlwriter
xsl
Zend OPcache
zip
zlib

[Zend Modules]
Xdebug
Zend OPcache
```
```
memory_limit = 256M
date.timezone = Europe/Paris
```

**Redis**
```
$ redis-server --version
Redis server v=3.2.7 sha=00000000:0 malloc=jemalloc-4.4.0 bits=64 build=2860cc704b521802
```

**RabbitMQ**
```
$ sudo service rabbitmq-server status
...
RabbitMQ 3.6.6. Copyright (C) 2007-2016 Pivotal Software, Inc.
...
```

**Javascript**

```
$node --version
v6.10.0
```

```
$npm --version
3.10.10
```

```
$gulp --version
CLI version 3.9.1
```

**ElasticSearch**
```
$ curl -XGET 'localhost:9200'
...
"number" : "1.7.6",
...
```

**Shell**
```
$ ps -p $$
  PID TTY          TIME CMD
 3371 pts/0    00:00:00 zsh
```

**Tools**
```
$ git --version
git version 2.1.4
```
```
$ composer --version
Composer version 1.3.2 2017-01-27 18:23:41
```
```
$ ansible --version
ansible 2.2.1.0
```
```
$ phantomjs --version
2.1.1
```
```
$ htop --version
htop 1.0.3 - (C) 2004-2011 Hisham Muhammad
Released under the GNU GPL.
```
```
$ screen --version
Screen version 4.02.01 (GNU) 28-Apr-14
```

**Additional config**

An environmental variable `ENV_VAGRANT` is set to ease the development : use it to add dev only features (debug, etc.)

Just check this variable with PHP (works with CLI and Apache) :
```php
<?php
if (getenv('ENV_VAGRANT')) {
    doSomeVagrantThing();
}
```
