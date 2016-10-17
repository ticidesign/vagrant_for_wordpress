# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/trusty64"
  config.vm.network "forwarded_port", guest: 80, host: 8080
  config.vm.synced_folder ".", "/vagrant"

  config.vm.provider "virtualbox" do |vb|
    # MySQL install fails if we use a smaller amount of RAM.
    vb.memory = "2048"
  end

  config.vm.provision "shell", inline: <<-SHELL
    # update / upgrade
    sudo apt-get update
    sudo apt-get upgrade -y

    # install apache 2.5 and php 5.5
    sudo apt-get install -y apache2 php5 php5-mcrypt

    # install mysql and give password to installer
    sudo debconf-set-selections <<< "mysql-server mysql-server/root_password password wordpress"
    sudo debconf-set-selections <<< "mysql-server mysql-server/root_password_again password wordpress"
    sudo apt-get install -y mysql-server php5-mysql

    # install phpmyadmin and give password(s) to installer
    sudo debconf-set-selections <<< "phpmyadmin phpmyadmin/dbconfig-install boolean true"
    sudo debconf-set-selections <<< "phpmyadmin phpmyadmin/app-password-confirm password wordpress"
    sudo debconf-set-selections <<< "phpmyadmin phpmyadmin/mysql/admin-pass password wordpress"
    sudo debconf-set-selections <<< "phpmyadmin phpmyadmin/mysql/app-pass password wordpress"
    sudo debconf-set-selections <<< "phpmyadmin phpmyadmin/reconfigure-webserver multiselect apache2"
    sudo apt-get install -y phpmyadmin

    # Link our files into where apache is expecting them.
    sudo rm -rf /var/www/html
    sudo ln -s /vagrant /var/www/html

    # Install the wp cli tool
    curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar
    chmod +x wp-cli.phar
    sudo mv wp-cli.phar /usr/local/bin/wp

    # enable mod_rewrite
    sudo a2enmod rewrite

    # Overwrite the default apache conf file to have the options we need.
    cat > /etc/apache2/sites-available/000-default.conf <<EOF
    <VirtualHost *:80>
      ServerName localhost
      ServerAdmin webmaster@localhost
      DocumentRoot /var/www/html
      <Directory "/var/www/html">
          AllowOverride All
      </Directory>

      # Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
      # error, crit, alert, emerg.
      # It is also possible to configure the loglevel for particular
      # modules, e.g.
      #LogLevel info ssl:warn

      ErrorLog ${APACHE_LOG_DIR}/error.log
      CustomLog ${APACHE_LOG_DIR}/access.log combined
    </VirtualHost>

    # vim: syntax=apache ts=4 sw=4 sts=4 sr noet
EOF

    # restart apache
    sudo service apache2 restart

    echo "=========================================="
    echo " Importing data. This will take a while."
    echo "=========================================="

    cd /var/www/html/
    wp --allow-root db create
    wp --allow-root core install --url=http://localhost:8080/ --title=ACRF --admin_user=admin --admin_password=admin --admin_email=admin@make.com.au
    # wp --allow-root plugin activate wordpress-importer
    # wp --allow-root import /var/www/html/beatoncapital.wordpress.2016-01-15.xml --authors=create

    echo "=========================================="
    echo " Ok! Your ACRF environment is ready."
    echo " Go to http://localhost:8080 to access."
    echo "=========================================="

  SHELL
end
