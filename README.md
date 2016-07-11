#1. Launch the Virtual Machine

* Public IP Address: 52.40.200.116

#2. Create a new user named grader and grant this user sudo permissions.

* Create a new yser named grader
```sudo adduser grader```


3. Update all currently installed packages.

4. Configure the local timezone to UTC.

5. Change the SSH port from 22 to 2200

6. Configure the Uncomplicated Firewall to only allow incoming connections.

7. Install and configure Apache to serve a Python mod_wsgi application

8. Install and configure PostgreSQL
    a. Do not allow remote connections
    b. Create a new user named catalog that has limited permissions to your catalog application database

9. Install git, clone and set up your Catalog App project (from your GitHub repository from earlier in the Nanodegree program) so that it functions correctly when visiting your server’s IP address in a browser. Remember to set this up appropriately so that your .git directory is not publicly accessible via a browser!

10. Your Amazon EC2 Instance's public URL will look something like this: http://ec2-XX-XX-XXX-XXX.us-west-2.compute.amazonaws.com/ where the X's are replaced with your instance's IP address. You can use this url when configuring third party authentication. Please note the the IP address part of the AWS URL uses dashes, not dots.
