Vagrant CentOS 6.5 (x86_64) LAMP+
====================================

This is my base dev setup.  Includes PHP, MySQL, Ruby, Python and phpMyAdmin

## Install VirtualBox
https://www.virtualbox.org/

## Install Vagrant (Requires 1.5+ for Vagrant Cloud)
http://www.vagrantup.com/

## Setup a New Vagrant VM
We'll be using my shared box on Vagrant Cloud. Vagrant will download the box and initialize your vagrant directory, including a shiny new Vagrantfile

```
mkdir your-project-dir && cd your-project-dir
vagrant init kevindeleon/centos65-x86_64-LAMP
vagrant up
vagrant ssh
```

## Starting Apache and MySQL

While SSH'd into your Vagrant VM:

```
sudo service httpd start
sudo service mysqld start
```

## A Few Vagrantfile Tweaks I Prefer

If you would like to be able to view content in your host machine's browser, I recommend setting your 'forwarded_port' in your Vagrantfile:

```ruby
config.vm.network "forwarded_port", guest: 80, host: 8080
```

I also create a sub-folder on my host machine (in the initialized vagrant directory) called 'html' and use the 'synced_folder' config in my Vagrantfile to sync it with '/var/www/html' on the guest machine.

```ruby
config.vm.synced_folder "html", "/var/www/html"
```

So, my folder structure on my host machine looks something like:
```
initialized-vagrant-folder/
--.vagrant/
--html/
--Vagrantfile
```

Remember, if you make changes to your Vagrantfile while your VM is running, you will need to issue the following command:

```
vagrant reload
```

After reloading your Vagrant box, start your services (Apache and MySQL) on your Vagrant box and you should be able to go to http://localhost:8080 and see your html/php in your web root. PhpMyAdmin is located at http://localhost:8080/phpMyAdmin/ (if you followed the port forwarding instructions above...if you set it to something other than 8080, you would obviously substitute that).

## A few things:
 * Your web root is /var/www/html (where your html/php files go)
 * I made changes to the phpMyAdmin config so that IPs other than 127.0.0.1 could connect. If you would like that security measure turned back on, simply replace /etc/httpd/conf.d/phpMyAdmin.conf with /etc/httpd/conf.d/phpMyAdmin.conf.OrigBak 
 * Currently MySQL is using 'root' as the root password. You may want to change that.

That's it!  Enjoy your new dev environment.
