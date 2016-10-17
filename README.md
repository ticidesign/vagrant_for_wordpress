# Wordpress Website Development Environment

To get started, clone this repository along with [Vagrant](https://www.vagrantup.com/) and 
[VirtualBox](https://www.virtualbox.org/) (for Mac).

Then run to start the virtual machine. This will also provision the machine with the 
configured software for Wordpress.

```
vagrant up --provision
```

Wordpress should now be accessible from port 8080 on the local machine.

The database is self contained and Wordpress code is shared from the www/wordpress directory 
in this repository.