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
$ wget http://wordpress.org/latest.tar.gz && tar xfz latest.tar.gz && mv wordpress/* ./ && rm -r wordpress/ && rm latest.tar.gz
```

# Start php/wordpress development server

```shell
$ php -S localhost:8000
```

And then click through wordpress five minute installation forms.

# Install Podlove Publisher Beta

Install Podlove Publisher Stable via wordpress plugins menu item.
Install and activate all 3 podlove plugins. Go to the Settings -> Permalinks
page and switch the permalink structure to "Post name". At this point
this endpoint should work: `http://localhost:8000/wp-json/podlove/v2/episodes/`.

Download Beta:

```shell
$ wget https://github.com/podlove/podlove-beta-tester/archive/master.zip
```

Upload + activate Beta in wordpress admin. Go to settings -> Podlove Beta Tester
and switch Beta to "on". Then update podlove publisher plugin. This does not work
if you have not installed the stable version before.

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

- If you don't change the permalink format, api endpoints wont work
- If you only upload the beta, podlove and episode menu items are missing 
- After changing the upload location, clicking verify does not work (although it shows the correct url and duration is set automatically), you have to change the slug - this is probably a bug in podlove, ah verify_all works, too
- Using files from wordpress media library did not work

# Cleanup

If you want to reinstall wordpress, you need to remove the wordpress
files.

```shell
$ rm -r mariadb/ wp-admin/ wp-content/ wp-includes/ xmlrpc.php index.php license.txt readme.html wp-*.php
```

## Podlovers.org Static Site Generator

Checkout project and install via npm:

```shell
$ git clone https://github.com/podlove/podlovers.org.git
$ cd podlovers.org
$ npm install
```

- `env NODE_APP_INSTANCE=podlovers npm run build` does not work
- `env NODE_APP_INSTANCE=forschergeist npm run build` use this config as a template for your own project

Make sure to use an url as poster image for your podcast in publisher, if
you upload an image `npm run build` just hangs. The poster image url is
published by the api and this url does not work. Either timeout or the php
dev server crashes.