# Linux_Server

This is a linux-server-configuration project 

## Description

Take the baseline installation of a Linux distribution on a virtual machine
Us the VM to prepare it to host your web applications, include installing updates,
and securing it from attack vectors, and installing/configuring web and database
servers.

    IP address: 

    Accessible SSH port: 2200

    Application URL: 
### How to Run

* Create a new user named _grader_ and give it the permission to _sudo_
* SSH into the server through:
```
ssh -i ~/.ssh/udacity_key.rsa root@35.167.27.204
```
* Run 
```
$ sudo adduser grader 
```
to create a new user named _grader_
* Create a new file in the sudoers directory with:
```
sudo nano /etc/sudoers.d/grader
```
* Add the following text grader ALL=(ALL:ALL) ALL
* Run 
```
sudo nano /etc/hosts
```
* Prevent the error sudo: unable to resolve host by adding this line 
```
127.0.1.1 ip
```
* Update all currently installed packages
* Download package lists with 
```
sudo apt-get update
```
* Fetch the new versions of packages using
```
sudo apt-get upgrade
```
* Change SSH port from 22 to 2200
* Then run
```
sudo nano /etc/ssh/sshd_config
```
* Change the port from 22 to 2200
* Confirm by running 
```
ssh -i ~/.ssh/udacity_key.rsa -p 2200 root@35.167.27.204
```
* Configure the Uncomplicated Firewall (UFW) to only allow incoming connections for SSH (port 2200), HTTP (port 80), and NTP (port 123)
```
    sudo ufw allow 2200/tcp
    sudo ufw allow 80/tcp
    sudo ufw allow 123/udp
    sudo ufw enable
```
* Configure the local timezone to UTC

* Run
```
sudo dpkg-reconfigure tzdata
```
* Then choose UTC
* Configure the key-based authentication for _grader user_
* Then run this command
```
cp /root/.ssh/authorized_keys /home/grader/.ssh/authorized_keys
```
* Disable ssh login for root user
* Then run
```
sudo nano /etc/ssh/sshd_config
```
* Change _PermitRootLogin without-password_ line to _PermitRootLogin no_
* Restart ssh with 
```
sudo service ssh restart
```
* Now you are only allowed to login using
```
ssh -i ~/.ssh/udacity_key.rsa -p 2200 grader@35.167.27.20
```
* Install Apache using the command
```
sudo apt-get install apache2
```
* Next, install mod_wsgi using the command:
```
   sudo apt-get install libapache2-mod-wsgi python-dev
```
* Enable thenmod_wsgi by running 
```
sudo a2enmod wsgi
```
* Now start the web server with 
```
sudo service apache2 start
```
* Then clone the item_catalog App from Github
* Install git using: 
```
sudo apt-get install git
cd /var/www
sudo mkdir catalog
```
* Then change the owner of the newly created item_catalog folder using
```
sudo chown -R grader:grader item_catalog
cd /item_catalog
```
* Next, clone your project from github using 
```
git clone https://github.com/rrjoson/udacity-item-catalog.git item_catalog
```
* Create a catalog.wsgi file, then add this inside:
```
import sys
import logging
logging.basicConfig(stream=sys.stderr)
sys.path.insert(0, "/var/www/item_catalog/")
```
* From the item_catalog, import app as application
```
application.secret_key = 'supersecretkey'
```
* Rename the _application.py_ to _init.py_ mv application.py __init__.py
* Install the virtual environment as follows:
    * Install the virtual environment ```sudo pip install virtualenv```
    * Create a new virtual environment with ```sudo virtualenv venv```
    * Activate the virutal environment source ```venv/bin/activate```
    * Change permissions ```sudo chmod -R 777 venv```

* Next, install Flask and other dependencies using:

    * Install pip with ```sudo apt-get install python-pip```
    * Install Flask ```pip install Flask```
    * Install other project dependencies ```sudo pip install httplib2 oauth2client sqlalchemy psycopg2 sqlalchemy_utils```

* Then update path of client_secrets.json file
```nano __init__.py```
* Change client_secrets.json path to /var/www/catalog/catalog/client_secrets.json
* Configure and enable a new virtual host
* Run this: 
```
sudo nano /etc/apache2/sites-available/catalog.conf
```
* Then paste this code:
```
<VirtualHost *:80>
    ServerName 35.167.27.204
    ServerAlias ec2-35-167-27-204.us-west-2.compute.amazonaws.com
    ServerAdmin admin@35.167.27.204
    WSGIDaemonProcess catalog python-path=/var/www/catalog:/var/www/item_catalog/venv/lib/python2.7/site-packages
    WSGIProcessGroup catalog
    WSGIScriptAlias / /var/www/item_catalog/item_catalog.wsgi
    <Directory /var/www/item_catalog/item_catalog/>
        Order allow,deny
        Allow from all
    </Directory>
    Alias /static /var/www/item_catalog/item_catalog/static
    <Directory /var/www/item_catalog/item_catalog/static/>
        Order allow,deny
        Allow from all
    </Directory>
    ErrorLog ${APACHE_LOG_DIR}/error.log
    LogLevel warn
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
* Next, enable the virtual host by running ```sudo a2ensite catalog```
* Install and configure PostgreSQL using:
```
    sudo apt-get install libpq-dev python-dev
    sudo apt-get install postgresql postgresql-contrib
    sudo su - postgres
    psql
    CREATE USER catalog WITH PASSWORD 'password';
    ALTER USER catalog CREATEDB;
    CREATE DATABASE catalog WITH OWNER catalog;
    \c catalog
    REVOKE ALL ON SCHEMA public FROM public;
    GRANT ALL ON SCHEMA public TO catalog;
    \q
    exit
    Change create engine line in your __init__.py and database_setup.py to: engine = create_engine('postgresql://catalog:password@localhost/item_catalog')
    python /var/www/item_catalog/item_catalog/database_setup.py
    Make sure no remote connections to the database are allowed. Check if the contents of this file sudo nano /etc/postgresql/9.3/main/pg_hba.conf looks like this:

local   all             postgres                                peer
local   all             all                                     peer
host    all             all             127.0.0.1/32            md5
host    all             all             ::1/128                 md5
```
* Now, restart the Apache using
```
   sudo service apache2 restart
```
    Visit site at http://35.167.27.204

# Resource

**Choose A License** - Helpful website for picking out a license for an application.
**Udacity** - Helpful in learning contents and resources to guide.

## How to Contribute

Contributions are welcomes. If you find any typos, errors,
or additional resources. First, fork this repository.
- You can Fork Icon the project.
- Or clone the repository to make changes.

```
$ git clone {REPOSITORY_CLONE_URL}

$ cd to the directory
```

- You can also Pull Request Icon by:

Making a pull request. Once you've pushed changes to
your local repository, you can issue a pull request
by clicking on the pull request icon.

# License

The contents of this repository are covered under the **MIT License**
