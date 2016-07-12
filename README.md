# 1\. Launch the Virtual Machine

- Public IP Address: 52.40.200.116

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

- On your local machine, generate an id and public key `$ ssh-keygen -f ~/.ssh/grader_key`
- `# mkdir /home/grader/.ssh`
- `# nano /home/grader/.ssh/authorized_keys`
- copy content of "grader_key.pub" on the local to "authorized_keys" on the host
- `sudo chmod 700 /home/grader/.ssh`
- `sudo chmod 644 /home/grader/.ssh/authorized_keys`
- `sudo chown -R grader:grader /home/grader/.ssh`

- `# sudo nano /etc/ssh/sshd_config` Find "Port" and modify 22 to 2200

- `# sudo service ssh restart`

- `$ ssh -i ~/.ssh/grader_key -p 2200 grader@52.40.200.116`

# 6\. Configure the Uncomplicated Firewall to only allow incoming connections.

- `# sudo ufw allow 2200/tcp`
- `# sudo ufw allow 80/tcp`
- `# sudo ufw allow 123/udp`
- `# sudo ufw enable`

# 7\. Install and configure Apache to serve a Python mod_wsgi application

- `# sudo apt-get install apache2`

- <http://ec2-52-40-200-116.us-west-2.compute.amazonaws.com/> sudo apt-get install libapache2-mod-wsgi got error when

  - Restarting web server apache2 AH00557: apache2: apr_sockaddr_info_get() failed for ip-10-20-5-126 AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using 127.0.0.1\. Set the 'ServerName' directive globally to suppress this message

- # sudo nano /etc/apache2/sites-enabled/000-default.conf

  add the following line at the end of the

  <virtualhost *:80=""> block, right before the closing </virtualhost>

  line: WSGIScriptAlias / /var/www/html/myapp.wsgi

- sudo apache2ctl restart same error occured! sudo nano /etc/apache2/conf-available/servername.conf ServerName localhost sudo a2enconf servername sudo apache2ctl restart sudo nano /var/www/html/myapp.wsgi

- Install and configure PostgreSQL a. Do not allow remote connections b. Create a new user named catalog that has limited permissions to your catalog application database

- Install git, clone and set up your Catalog App project (from your GitHub repository from earlier in the Nanodegree program) so that it functions correctly when visiting your server's IP address in a browser. Remember to set this up appropriately so that your .git directory is not publicly accessible via a browser!

- Your Amazon EC2 Instance's public URL will look something like this: <http://ec2-XX-XX-XXX-XXX.us-west-2.compute.amazonaws.com/> where the X's are replaced with your instance's IP address. You can use this url when configuring third party authentication. Please note the the IP address part of the AWS URL uses dashes, not dots.
