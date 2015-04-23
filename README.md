# nanodegree-sysadmin

### PROJECT 5: set up the server

```
# ADD a user 'grader'
$> adduser grader

# ADD the user to the sudo group
$> adduser grader sudo

# UPDATE all packages
$> apt-get update
$> apt-get upgrade
# if you get any error during update try: '$> sudo dpkg --configure -a' to find out

# RUN ssh daemon on port 2200:
# 1. Set 'Port' in /etc/ssh/sshd_config to '2200'
Port 2200
# 2. Restart the ssh daemon
$> restart ssh

# SET local time to UTC
$> dpkg-reconfigure tzdata
# 1. select 'Others' or 'None of the above'
# 2. select 'UTC' 

# DEFINE an authorized key for user 'grader'
# 1. create a '.ssh' directory in the user's home,  SSH is picky about files permissions
$> mkdir /home/grader/.ssh
$> chown grader /home/grader/.ssh
$> chmod 700 /home/grader/.ssh
# 2. create a file to store authorized keys
$> cd /home/grader/.ssh
$> touch authorized_keys
# 3. copy-paste your public key in the 'authorized_keys' file
# 4. SSH is picky about files permissions: 
$> chmod 600 authorized_keys
$> chown grader authorized_keys
# Now you can login from the client via ssh on port 2200 by using your key and the user grader:
# $> ssh -p 2200 grader@THE.VM.IP.NUMBER

# DISABLE root login from remote
# 1. Set 'permitRootLogin' in /etc/ssh/sshd_config to 'no'
permitRootLogin no
# 2. Add a line with 'AllowUsers grader'
AllowUsers grader
# 3. restart ssh
$> restart ssh
# Now you can login via ssh only as grader user and your key

# ENABLE universal firewall for given ports
$> sudo ufw allow 2200
$> sudo ufw allow 80
$> sudo ufw allow 123
$> sudo ufw enable
# check the active rules:
$> sudo ufw status verbose

# INSTALL webserver and WSGI
# 1. install Apache 2
$> sudo apt-get install apache2
# you can check now your Apache server working at http://THE.VM.IP.NUMBER
# 2. install mod_wsgi
$> sudo apt-get install python-setuptools libapache2-mod-wsgi python-dev
$> sudo service apache2 restart

# INSTALL POSTgre
# 1. install the package
$> sudo apt-get install postgresql
# 2. check if service is running
$> sudo service postgresql status

# POSTgre management
# 1. login as postgres user into psql to have administrative rights on the database
$> su - postgres
$> psql

```

Useful Links:
http://bullium.com/support/vim.html<br>
https://help.ubuntu.com/community/UFW<br>


### Running the app:

Git clone this repository:
```
$ git clone https://github.com/Mec-iS/nanodegree-fullstack-final
```

Install the necessary python libraries:
```
$ pip install -r requirements.txt
```

Add your GitHub OAUTH tokens in `libs\secret.py`.<br>
Change your database username and password in `libs\database_setup.py`.


Change directory to repository directory and:.

* Run the `lotofmenus.py` script in `tests`, to create the database and tables, and load some mock data.

* Run the server by running the `finalproject.py` script

Visit `127.0.0.1:5000` on your browser.



<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Licenza Creative Commons" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a>