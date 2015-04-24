# nanodegree

## PROJECT 5: set up the server

VM IP: 54.201.214.146

### ADD a user 'grader'
```
$> adduser grader
```
### ADD the user to the sudo group
```
$> adduser grader sudo
```
### UPDATE all packages
```
$> apt-get update
$> apt-get upgrade
# if you get any error during update try: '$> sudo dpkg --configure -a' to find out
```
### RUN ssh daemon on port 2200:
#### 1. Set 'Port' in /etc/ssh/sshd_config to '2200'
```
Port 2200
```
#### 2. Restart the ssh daemon
```
$> restart ssh
```
### SET local time to UTC
```
$> dpkg-reconfigure tzdata
```
#### 1. select 'Others' or 'None of the above'
#### 2. select 'UTC' 

### DEFINE an authorized key for user 'grader'
#### 1. create a '.ssh' directory in the user's home,  SSH is picky about files permissions
```
$> mkdir /home/grader/.ssh
$> chown grader /home/grader/.ssh
$> chmod 700 /home/grader/.ssh
```
#### 2. create a file to store authorized keys
```
$> cd /home/grader/.ssh
$> touch authorized_keys
```
#### 3. copy-paste your public key in the 'authorized_keys' file
#### 4. SSH is picky about files permissions: 
```
$> chmod 600 authorized_keys
$> chown grader authorized_keys
# Now you can login from the client via ssh on port 2200 by using your key and the user grader:
# $> ssh -p 2200 grader@THE.VM.IP.NUMBER
```
### DISABLE root login from remote
#### 1. Set 'permitRootLogin' in /etc/ssh/sshd_config to 'no'
```
permitRootLogin no
```
#### 2. Add a line with 'AllowUsers grader'
```
AllowUsers grader
```
#### 3. restart ssh
```
$> restart ssh
# Now you can login via ssh only as grader user and your key
```
### ENABLE universal firewall for given ports
```
$> sudo ufw allow 2200
$> sudo ufw allow 80
$> sudo ufw allow 123
$> sudo ufw enable
# check the active rules:
$> sudo ufw status verbose
```
### INSTALL webserver and WSGI
#### 1. install Apache 2
```
$> sudo apt-get install apache2
# you can check now your Apache server working at http://THE.VM.IP.NUMBER
```
#### 2. install mod_wsgi
```
$> sudo apt-get install python-setuptools libapache2-mod-wsgi python-dev
$> sudo service apache2 restart
```
#### 3. Check if wsgi is enabled
```
$> sudo a2enmod wsgi 
```
### INSTALL POSTgre
#### 1. install the package
```
$> sudo apt-get install postgresql
```
#### 2. check if service is running
```
$> sudo service postgresql status
```
### POSTgre management
#### 1. login as postgres user into psql to have administrative rights on the database
```
$> su - postgres
$> psql
psql> CREATE DATABASE catalog;
psql> CREATE USER catalog PASSWORD 'somepwd';
psql> GRANT ALL ON DATABASE catalog TO catalog;
```
### INSTALL git
```
$> sudo apt-get install git
```
### Install PSYCOPG2
```
# substitute X.X with your POSTgre version
$> sudo apt-get install postgresql-server-dev-X.X
$> sudo pip install psycopg2==2.6
```
### CLONE the app
```
$> cd /var/www
$> sudo mkdir wsgi
$> cd wsgi
$> sudo git clone https://github.com/Mec-iS/nanodegree
```
### Add the WSGI entrypoint
#### create the application.wsgi file in the wsgi/ directory with this content:
```
#!/usr/bin/python
import sys
import logging
logging.basicConfig(stream=sys.stderr)
sys.path.insert(0,"/var/www/wsgi/")

from nanodegree.libs.secret import secret_key

from nanodegree.finalproject import app as application
application.secret_key = secret_key
```

### RUN flask
```
$> cd /var/www/wsgi/nanodegree
```
#### 1. install dependencies
```
$> sudo python -m pip install -r requirements.txt
# change user and password for POSTgre in 'libs/database_setup.py' 
# change the github app's keys in 'libs/secrets.py'
```
#### 2. check if server runs properly
```
$> python finalproject.py
# if the server starts, everything is ok
```
#### 3. Create a virtual host for Apache2 with this content:
```
<VirtualHost *:80>
                ServerName WSGIserver
                ServerAdmin admin@mywebsite.com
                WSGIScriptAlias / /var/www/wsgi/application.wsgi
                <Directory /var/www/wsgi/nanodegree>
                        Order allow,deny
                        Allow from all
                </Directory>
                Alias /static /var/www/wsgi/nanodegree/static
                <Directory /var/www/wsgi/nanodegree/static/>
                        Order allow,deny
                        Allow from all
                </Directory>
                ErrorLog ${APACHE_LOG_DIR}/error.log
                LogLevel warn
                CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
### 4. Check if there is something missing 
```
# check if everything is fine as explained 
# here: https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps
# at steps four and five:
$> sudo service apache2 restart
```


Useful Links:

http://bullium.com/support/vim.html<br>
https://help.ubuntu.com/community/UFW<br>
http://discussions.udacity.com/t/p5-how-i-got-through-it/15342<br>
http://discussions.udacity.com/t/submission-format/4006/2<br>
http://askubuntu.com/a/428034<br>
http://www.ece.uci.edu/~chou/ssh-key.html<br>
http://stackoverflow.com/q/21084791<br>



<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Licenza Creative Commons" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a>