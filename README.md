# 1\. Launch the Virtual Machine

- Public IP Address: 52.40.200.116
- URL: <http://ec2-52-40-200-116.us-west-2.compute.amazonaws.com/>

# 2\. Create a new user named grader and grant this user sudo permissions.

- Create a new user named "grader" and set password `# sudo adduser grader`
- Create a new file named "grader" in "sudoers.d" directory `# sudo nano /etc/sudoers.d/grader`
- Set following text and save it `grader ALL=(ALL:ALL) ALL`

# 3\. Update all currently installed packages.

- `# sudo apt-get update`
- `# sudo apt-get upgrade`
- `# apt-get install finger`

# 4\. Configure the local timezone to UTC.

- Config and set local time to UTC `# sudo dpkg-reconfigure tzdata`
- Install ntp `# sudo apt-get install ntp`

# 5\. Change the SSH port from 22 to 2200

- On your local machine, generate an id and public key `$ ssh-keygen -f ~/.ssh/udacity_key.rsa`
- `# mkdir /home/grader/.ssh`
- `# nano /home/grader/.ssh/authorized_keys`
- copy content of "udacity_key.rsa.pub" on the local to "authorized_keys" on the host
- `sudo chmod 700 /home/grader/.ssh`
- `sudo chmod 644 /home/grader/.ssh/authorized_keys`
- `sudo chown -R grader:grader /home/grader/.ssh`

- `# sudo nano /etc/ssh/sshd_config` Find "Port" and modify 22 to 2200

- `# sudo service ssh restart`

- `$ ssh -i ~/.ssh/udacity_key.rsa -p 2200 grader@52.40.200.116`

# 6\. Configure the Uncomplicated Firewall to only allow incoming connections.

- `# sudo ufw allow 2200/tcp`
- `# sudo ufw allow 80/tcp`
- `# sudo ufw allow 123/udp`
- `# sudo ufw enable`

# 7\. Install "fail2ban" to monitor for repeated unsuccessful login attempts and ban attackers

- `# sudo apt-get install fail2ban`
- "sendmail" package to send the alerts to the admin user `# sudo apt-get install sendmail`
- `# sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local`
- `# sudo nano /etc/fail2ban/jail.local` And set the "destemail" field to admin user's email address

# 8\. Install and configure Apache to serve a Python mod_wsgi application

- Install apache2 `# sudo apt-get install apache2`

- <http://ec2-52-40-200-116.us-west-2.compute.amazonaws.com/>

- Install mod-wsgi `# sudo apt-get install libapache2-mod-wsgi python-dev`

- Enable mod_wsgi: `# sudo a2enmod wsgi`

- `# sudo service apache2 start`

# 9\. Install GitHub

- `# sudo apt-get install git`
- `# git config --global user.name <username>`
- `# git config --global user.email <email>`

# 10\. Clone Catalog from GitHub

- Create Catalog directory and change ownership `# cd /var/www

  # sudo mkdir catalog

  # sudo chown -R grader:grader catalog`

- Clone Catalog repo from GitHub `# cd catalog

  # git clone <https://github.com/halee9/catalog.git> catalog`

- Create "catalog.wsgi" under "catalog" folder and set: ```Python

import sys import logging logging.basicConfig(stream=sys.stderr) sys.path.insert(0, "/var/www/catalog/")

from catalog import app as application`

```

# 11\. Create a new virtual host

- Create a virtual host conifg file `# sudo nano /etc/apache2/sites-available/catalog.conf`
-
```

<virtualhost *:80="">
    ServerName 52.40.200.116
    ServerAlias ec2-52-40-200-116.us-west-2.compute.amazonaws.com
    ServerAdmin admin@52.40.200.116
    WSGIDaemonProcess catalog python-path=/var/www/catalog:/var/www/catalog/venv/lib/python2.7/site-packages
    WSGIProcessGroup catalog
    WSGIScriptAlias / /var/www/catalog/catalog.wsgi
    <directory var="" www="" catalog="">
</directory></virtualhost>

```
  Order allow,deny
    Allow from all
```

Alias /static /var/www/catalog/catalog/static

<directory var="" www="" catalog="" static="">
  <pre>

  <code>  Order allow,deny
    Allow from all
  </code>
</pre>
</directory>

ErrorLog ${APACHE_LOG_DIR}/error.log LogLevel warn CustomLog ${APACHE_LOG_DIR}/access.log combined ```

- sudo a2dissite 000-default

- sudo a2endsite catalog

# 12

- Installing Python packages `# sudo apt-get install python-pip`
- sudo pip install virtualenv
- cd /var/www/catalog
- sudo virtualenv venv
- source venv/bin/activate
- sudo chmod -R 777 venv
- pip install Flask
- pip install bleach httplib2 request oauth2client sqlalchemy psycopg2

# 13 Install and configure PostgreSQL

- sudo apt-get install libpq-dev python-dev
- sudo apt-get install postgresql postgresql-contrib
- sudo su - postgres
- psql `CREATE USER catalog WITH PASSWORD 'password'; ALTER USER catalog CREATEDB; CREATE DATABASE catalog WITH OWNER catalog; \c catalog; REVOKE ALL ON SCHEMA public FROM public; GRANT ALL ON SCHEMA public TO catalog; \q`
- exit engine = create_engine('postgresql://catalog:password@localhost/catalog')
- python /var/www/catalog/catalog/catalog_db.py

sudo service apache2 restart
