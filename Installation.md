# Install PHP

Wordpress requires PHP to be installed.

```shell
$ brew install php
```

```shell
$ php --version                              1.6m  Do 21 Apr 12:07:26 2022
PHP 8.1.5 (cli) (built: Apr 16 2022 00:03:52) (NTS)
Copyright (c) The PHP Group
Zend Engine v4.1.5, Copyright (c) Zend Technologies
    with Zend OPcache v8.1.5, Copyright (c), by Zend Technologies
```

# Install MariaDB

Install mariaDB using homebrew:

```shell
$ brew install mariadb
```

Correct version installed?
```shell
$ mysqld --version
mysqld  Ver 10.7.3-MariaDB for osx10.17 on arm64 (Homebrew)
```

Create the mariadb database directory:
```shell
$ mkdir mariadb
```

Initialize mariaDB:

```shell
$ mysql_install_db --user=jochen --datadir=./mariadb
```

Start mariaDB:

```shell
/opt/homebrew/Cellar/mariadb/10.7.3/bin/mysqld_safe --datadir='./mariadb'
```

Create wordpress database:

```shell
$ mysql -u jochen -e "create database wordpress;"
```

# Install Wordpress

Download tarball? wtf..

```shell
$ wget http://wordpress.org/latest.tar.gz
$ tar xfz latest.tar.gz
$ mv wordpress/* ./
$ rm -r wordpress/
$ rm latest.tar.gz
```

# Start php/wordpress development server

```shell
$ php -S localhost:8000
```

And then click through wordpress five minute installation forms.

# Install Podlove Publisher Beta

Download Beta:

```shell
$ wget https://github.com/podlove/podlove-beta-tester/archive/master.zip
```

Upload + activate Beta in wordpress admin. Go to settings -> Podlove Beta Tester
and switch Beta to "on".

```shell
$ rm master.zip
```

# Caveats

## Podlovers.org npm install

Had to install vips via homebrew, because there are no pre-build binaries
for Apple M1.

```shell
$ brew install vips
```

Whoa vips is a hell of a dependency (pulled 500MB qt for example :().

## PHP max Upload File Size

The default is 2M, which does not make sense for podcasts. Increase
it by modifying the php.ini file `/opt/homebrew/etc/php/8.1/php.ini`

```php
upload_max_filesize = 256M
post_max_size = 256M
```

## Podlove Publisher

- If you only upload the beta, podlove and episode menu items are missing 
- Episodes are only savable after podcast settings have been completed
- After changing the upload location, clicking verify does not work, you have to change the slug - this is probably a bug in podlove
- Still haven't found the api :(

# Cleanup

If you want to reinstall wordpress, you need to remove the wordpress
files.

```shell
$ rm index.php license.txt readme.html wp-*
$ rm -r wp-admin/ wp-content/ wp-includes/ xmlrpc.php
```