
## Linux Server Configuration
This is the final project for Udacity's Full Stack Web Developer Nanodegree.

This page explains how to secure and set up a Linux distribution on a virtual machine, install and configure a web and database server to host a web application.

The Linux distribution is Ubuntu 16.04 LTS.
The virtual private server is Amazon Lighsail.
The web application is my Item Catalog project created earlier in this Nanodegree program.
The database server is PostgreSQL.

You can visit http://54.174.92.92/ for the website deployed.

## Step 1: Start a new Ubuntu Linux server instance on Amazon LightsailLogin into Amazon Lightsail using an Amazon Web Services account.
  1. Once you are login into the site, click Create instance.Choose Linux/Unix platform, OS Only and Ubuntu 16.04 LTS.
  2. Choose a instance plan (I took the cheapest, $5/month).Keep the default name provided by AWS or rename your instance.Click the Create button to create the instance.Wait for the instance to start up.
## Step 2: SSH into the server
  1. From the Account menu on Amazon Lightsail, click on SSH keys tab and download the Default Private Key.
  2. Move this private key file named LightsailDefaultPrivateKey-.pem into the local folder ~/.ssh and rename it lightsail_key.rsa.*
  3. In your terminal, type: chmod 600 ~/.ssh/lightsail_key.rsa (gives owner (1) read (2) write (3) no execute
  4. To connect to the instance via the terminal: ssh -i ~/.ssh/lightsail_key.rsa ubuntu@54.174.92.92, where 54.174.92.92 is the public IP address of the instance.
## Step 3: Update and upgrade installed packages
  1. sudo apt-get updatesudo apt-get upgrade
## Step 4: Change the SSH port from 22 to 2200
  1. Edit the /etc/ssh/sshd_config file: sudo nano /etc/ssh/sshd_config.
  2. Change the port number on line 5 from 22 to 2200. Save and exit using CTRL+X and confirm with Y.
  3. Restart SSH: sudo service ssh restart.
## Step 5: Configure the Uncomplicated Firewall (UFW) to only allow incoming connections for SSH (port 2200), HTTP (port 80), and NTP (port 123)
  1. sudo ufw allow 2200/tcp (ports 1024 - 49151 are user or registered ports)
  2. sudo ufw allow 80/tcp (standard port for HTTP traffic)
  3. sudo ufw allow 123/udp (ports 0 - 1023 are system or well-known ports)
  4. sudo ufw enable
  5. Exit ssh connection: exit
  6. Verify UFW set up correctly by logging in:
    ssh -i ~/.ssh/lightsail_key.rsa -p 2200 ubuntu@54.174.92.92
## Step 6: Set up Grader
  1. Run the following commands:
    Sudo adduser grader
    Sudo visudo
  2. Give grader permission to “sudo”
      root ALL=(ALL:ALL) ALL
      grader  ALL=(ALL:ALL) ALL
  3. Verify grader has sudo permission
    su - grader
    sudo -l
  4. Create SSH key pair for grader using ssh-keygen
  5. On local machine
    ssh-keygen/Users/trantos/.ssh/grader_keycat ~/.ssh/grader_key.pub (copy content) and should look like something like this:
    ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDCDjr8BncD2yk8JBBT6BdsmzQKwgfjb836pWeysVC+esNFZSBa6EjDmlHLjfOgE6HcXWhQn+Tap+NkCp9Rp4u7dF0JopEYsYIPJqiRcrvV9cP77ct8hIaWpIQeRiv8qYlD2Ct4aPCbbRCFKuP0Ozjhcd+u06hlqSkic99+HD87GbuTW2ZIDyetYaHmdUAwQZ6EO3ITEQqBf8EYPbF9OHpz/7I8kkbE5N2Q6N9/IDv8d3dLP/w5m8wR8qxilcq7A1I7J3a1EoaTaLR60mVvq2mzkAx6WtjkeFxb6H8/fSnfj9Kxnu6PcpNYHH/fyONUxVll0uW/WsW1daPIyaf2qJxX trantos@Kendalls-MacBook-Pro.local

    a. Corresponding Server Side Private Key is below:
                -----BEGIN OPENSSH PRIVATE KEY-----
            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
            -----END OPENSSH PRIVATE KEY-----


  6. On grader’s virtual machine (su - grader):
  7. mkdir .ssh
  8. sudo nano ~/.ssh/authorized_keys (paste content)
  9. Give the permissions: chmod 700 .ssh (owner read/write/execute) and sudo chmod 644 .ssh/authorized_keys (owner read/write, other users read)
  10. Check in ‘/etc/ssh/sshd_config’ file if PasswordAuthentication is set to no
  11. On the local machine, run: ssh -i ~/.ssh/grader_key -p 2200 grader@54.174.92.92
  12. “Exit” once verified to back into local machine
## Step 7: Configure the local timezone to UTC
  1. While logged in as grader, configure the time zone: sudo dpkg-reconfigure tzdata.
  2. You should see something like that: 
    Current default time zone: 'America/Montreal'
    Local time is now: Thu Oct 19 21:55:16 EDT 2017.
    Universal Time is now: Fri Oct 20 01:55:16 UTC 2017.
## Step 8: Install and configure Apache to serve a Python mod_wsgi application
  1. While logged in as grader, install Apache: sudo apt-get install apache2. 
  2. Enter public IP of the Amazon Lightsail instance into browser. If Apache is working, you should see the default Apache2 Ubuntu Default page.
  3. My project is built with Python 3. So, I need to install the Python 3 mod_wsgi package:  sudo apt-get install libapache2-mod-wsgi-py3.
  4. Enable mod_wsgi using: sudo a2enmod wsgi. (Already enabled by default)
## Step 9: Install and configure PostgreSQL
  1. While logged in as grader, install PostgreSQL: sudo apt-get install postgresql.
  2. PostgreSQL should not allow remote connections. In the /etc/postgresql/9.5/main/pg_hba.conf file, you should see:
    IPv4 local connections:host    all             all             127.0.0.1/32            md5# IPv6 local connections:host    all             all             ::1/128                 md5# Allow replication connections from localhost, by a user with the# replication privilege.#local   replication     postgres                                peer#host    replication     postgres        127.0.0.1/32            md5#host    replication     postgres        ::1/128                 md5
  3. Switch to the postgres user: sudo su - postgres.
  4. Open PostgreSQL interactive terminal with psql.
  5. Create the catalog user with a password and give them the ability to create databases: 
    postgres=# CREATE ROLE catalog WITH PASSWORD ‘password’;
    postgres=# ALTER ROLE catalog CREATEDB;
  6. List the existing roles: du. The output should be like this: 
                                  List of roles
  Role name |                         Attributes                         | Member of-----------+------------------------------------------------------------+-----------catalog   | Create DB                                                  | {}postgres  | Superuser, Create role, Create DB, Replication, Bypass RLS | {}

  7. Exit psql: q
  8. Switch back to the grader user: exit. 
  9. Create a new Linux user called catalog: sudo adduser catalog.
  10. Enter password and fill out information.
  11. Give to catalog user the permission to sudo. Run: sudo visudo.
  12. Search for the lines that looks like this: 
    root    ALL=(ALL:ALL) ALL
    grader  ALL=(ALL:ALL) ALL
  13. Below this line, add a new line to give sudo privileges to catalog user.
 root    ALL=(ALL:ALL) ALL
    grader  ALL=(ALL:ALL) ALL
    catalog  ALL=(ALL:ALL) ALL
  14. Verify that catalog has sudo permissions.
  15. Run su - catalog, enter the password, run sudo -l and enter the password again.
  16. The output should be like this:
    Matching Defaults entries for catalog on ip-172-26-5-119.ec2.internal:env_reset, mail_badpass,secure_path=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin
    User catalog may run the following commands on ip-172-26-5-119.ec2.internal:(ALL : ALL) ALL
  17. While logged in as catalog, create a database: createdb catalog.
  18. Run psql and then run l to see that the new database has been created.
  19. The output should be like this: 
                          List of databases
    Name    |  Owner   | Encoding |   Collate   |    Ctype    |   Access privileges
    * -----------+----------+----------+-------------+-------------+-----------------------*  catalog   | catalog  | UTF8     | en_US.UTF-8 | en_US.UTF-8 |*  postgres  | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 |*  template0 | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/postgres          +*            |          |          |             |             | postgres=CTc/postgres  template1 | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/postgres          +*            |          |          |             |             | postgres=CTc/postgres* (4 rows)
  20. Exit psql: \q.
  21. Switch back to the grader user: exit
## Step 10: Install git and set up your Catalog App project.
  1. Install Git using sudo apt-get install git
  2. While logged in as grader, create sudo mkdir /var/www/FlaskApp/ directory. 
  3. Change to that directory and clone the catalog project: 
    sudo git clone https://github.com/kendalltran/pokemon.git FlaskApp. 
  4. From the /var/www directory, change the ownership of the FlaskApp directory to grader using: 
    sudo chown -R grader:grader FlaskApp/
  5. Change to the /var/www/FlaskApp/ directory.
  6. Create a catalog.wsgi file
    Touch catalog.wsgi
    Sudo nano catalog.wsgi
  7. Add this inside the file:
      #!/usr/bin/python
      import sys
      import logging
      logging.basicConfig(stream=sys.stderr)
      sys.path.insert(0,"/var/www/FlaskApp/")

      from FlaskApp import app as application
      application.secret_key = 'Add your secret key'
  8. Rename views.py to init.py: mv views.py __init__.py
## Step 11: Install the virtual environment and dependencies
1. While logged in as grader, install pip: sudo apt-get install python3-pip. 
2. Install the virtual environment: sudo apt-get install python-virtualenv 
3. Change to the /var/www/catalog/catalog/ directory. 
4. Create the virtual environment: sudo virtualenv -p python3 venv3 
5. Change the ownership to grader with: sudo chown -R grader:grader venv3/ 
6. Activate the new environment: . venv3/bin/activate
7. Install the following dependencies
  pip install -r requirements.txt
8. Run python3 __init__.py and you should see:
  Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
9. Deactivate the virtual environment: deactivate.

## Step 12: Set up and enable a virtual host
  1. Add the following line in /etc/apache2/mods-enabled/wsgi/conf file to use Python 3:

  #WSGIPythonPath: Additional directories to search for Python modules,
  #overriding the PYTHONPATH environment variable.
  #Used to specify additional directories to search for Python modules.
  #If multiple directories are specified they should be separated by a ':'.

      #WSGIPythonPath directory|directory-1:directory-2:...
      WSGIPythonPath /var/www/catalog/catalog/venv3/lib/python3.5/site-packages

  2. Create /etc/apache2/sites-available/catalog.conf ad add the following lines to configure the virtual host:

    <VirtualHost *:80>
            ServerName 54.174.92.92
            ServerAdmin kendalltran@gmail.com
            WSGIScriptAlias / /var/www/FlaskApp/flaskapp.wsgi
            <Directory /var/www/FlaskApp/FlaskApp/>
                    Order allow,deny
                    Allow from all
            </Directory>
            Alias /static /var/www/FlaskApp/FlaskApp/static
            <Directory /var/www/FlaskApp/FlaskApp/static/>
                    Order allow,deny
                    Allow from all
            </Directory>
            ErrorLog ${APACHE_LOG_DIR}/error.log
            LogLevel warn
            CustomLog ${APACHE_LOG_DIR}/access.log combined
    </VirtualHost>

  3. Enable virtual host: sudo a2ensite catalog.
  4. The following prompt will be returned: Enabling site catalog.
  5. To activate the new configuration, you need to run: service apache2 reload
  6. Reload Apache: sudo service apache2 reload.

## Step 13: Set up the database schema and populate the database_setup
  1. Edit /var/www/FlaskApp/FlaskApp/defaultusersandpokemon.py
  2. From the /var/www/FlaskApp/FlaskApp/ directory, activate the virtual environment
    . venv3/bin/activate
  3. Run: python3 database_setup.py
  4. Deactivate the virtual environment: deactivate

## Step 14: Disable the default Apache sites
  1. Disable the default Apache site: sudo a2dissite 000-default.conf. The following prompt will be returned: Site 000-default disabled.
  2. To activate the new configuration, you need to run:
    service apache2 reload
  3. Reload Apache: sudo service apache2 Reload

## Step 15:  Authenticate Login through Google
  1. Go to Google Cloud Platform:
    https://console.cloud.google.com/apis/dashboard?project=my-projects-1555531085094&folder=&organizationId=
  2. Click APIs & services on left menu.
  3. Click Credentials.
  4. Create an OAuth Client ID (under the Credentials tab), and add
    http://ec2-54-174-92-92.us-east-1a.compute.amazonaws.com as authorized JavaScript origins.
    Add http://ec2-54-174-92-92.us-east-1a.compute.amazonaws.com/oauth2callbackas authorized redirect URI.
  5. Download the corresponding JSON file, open it et copy the contents.
  6. Open /var/www/FlaskApp/FlaskApp/client_secrets.json and paste the previous contents into the this file.
  7. Replace the client ID to line 25 of the templates/login.html file in the project directory.

## Step 16:  Launch the Web application
  1. Change the ownership of the project directories:
    sudo chown -R www-data:www-data FlaskApp/
  2. Restart Apache again:
    sudo service apache2 reload
  3. Open your brower to http://54.174.92.92/
