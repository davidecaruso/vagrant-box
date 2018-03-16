# Vagrant Box

A generic Vagrant machine to create local websites.

| Service                   | Version | Info         |
| ------------------------- | ------- | -------      |
| Debian 8 Jessie           | 8       | --           |
| Apache                    | 2.4     | --           |
| PHP                       | 7.1     | --           |
| MariaDB                   | 10.0    | --           |
| PhpMyAdmin                | 4.7     | --           |
| IP address                | --      | 10.11.12.201 |
| Shared folders            | --      | /var/www     |

## Requirements
You need [Virtual Box](https://www.virtualbox.org/) (with the **Extension Pack**) and [Vagrant](https://www.vagrantup.com/) installed.

## Set up
Start the virtual machine (it will take a few minutes):
```
$ cd path/to/folder
$ vagrant up
```

Access to the machine, log in as root and install libraries:
```
$ vagrant ssh
$ sudo -i
$ apt-get update -y

$ # Install cURL, MariaDB, PMA, Vim, Drush (optional)
$ apt-get install -y curl mariadb-server mariadb-client phpmyadmin vim-nox drush

$ # Install PHP 7.1
$ apt-get install apt-transport-https lsb-release ca-certificates
$ apt-get update -y
$ apt-get install php7.1

$ # Provide the PMA host editing the apache2.conf file, adding the following line: 
$ # Include /etc/phpmyadmin/apache.conf
$ vim /etc/apache2/apache2.conf

$ # Edit the default host and add the following lines:
$ # <Directory /var/www/html>
$ #     AllowOverride All
$ # </Directory>
$ vim /etc/apache2/sites-enabled/000-default.conf

$ # Enable mod_rewrite and restart apache
$ a2enmod rewrite
$ service apache2 restart
```

Now visit [10.11.12.201](http://10.11.12.201/)

## Add virtual host
You can create custom virtual hosts, but you have to configure your virtual and your local machines.
#### Vagrant machine
Edit the Apache configuration file...

`$ sudo vim /etc/apache2/sites-available/000-default.conf`

...and add the following snippet replacing the **example.dev** text with your host name:

```
<VirtualHost *:80>
  # The ServerName directive sets the request scheme, hostname and port that
  # the server uses to identify itself. This is used when creating
  # redirection URLs. In the context of virtual hosts, the ServerName
  # specifies what hostname must appear in the request's Host: header to
  # match this virtual host. For the default virtual host (this file) this
  # value is not decisive as it is used as a last resort host regardless.
  # However, you must set it for any further virtual host explicitly.
  ServerName www.example.dev
  ServerAlias example.dev

  ServerAdmin webmaster@localhost
  DocumentRoot /var/www/html

  # Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
  # error, crit, alert, emerg.
  # It is also possible to configure the loglevel for particular
  # modules, e.g.
  #LogLevel info ssl:warn

  ErrorLog ${APACHE_LOG_DIR}/example.dev-error.log
  CustomLog ${APACHE_LOG_DIR}/example.dev-access.log combined

  # For most configuration files from conf-available/, which are
  # enabled or disabled at a global level, it is possible to
  # include a line for only one particular virtual host. For example the
  # following line enables the CGI configuration for this host only
  # after it has been globally disabled with "a2disconf".
  #Include conf-available/se`rve-cgi-bin.conf
</VirtualHost>
```
Save the file and restart the Apache service:

`$ service apache2 restart`

#### Local machine
Edit the *hosts* file
```
$ sudo vim /etc/hosts
$ # And add this record

10.11.12.201 example.dev
```

## Author

[Davide Caruso](https://davidecaruso.github.io)

## License

Licensed under [MIT](https://opensource.org/licenses/mit-license.php).
