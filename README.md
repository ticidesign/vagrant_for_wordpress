# Wordpress Website Development Environment

To get started, clone this repository along with [Vagrant](https://www.vagrantup.com/) and 
[VirtualBox](https://www.virtualbox.org/) (for Mac).

Then run to start the virtual machine. This will also provision the machine with the 
configured software for Wordpress.

```
vagrant up --provision
```

Wordpress should now be accessible from port 8080 on the local machine.

* * *

Vagrant PHP ADMIN

To access/sync phpmyadmin on host machine run this command on your terminal: ln -fs /usr/share/phpmyadmin /var/www