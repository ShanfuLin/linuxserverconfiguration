LINUX SERVER CONFIGURATION (UDACITY PROJECT FOR FSND STUDENTS)

Overview
Linux Server Configuration is a project that students of Udacity's Full Stack Nanodegree have to complete as part of their graduation criteria from the program.

A README file is included in the GitHub repo containing the following information: IP address, URL, summary of software installed, summary of configurations made, and a list of third-party resources used to complete this project.

   1. IP address

   http://54.254.165.218/

   2. Summary of software installed on Ubuntu (Lightsail)

   Python 2.7.12
   - Flask (web framework written in python)
   - Postgresql (object-relational database management system)
   - sqlalchemy (open source SQL toolkit and object-relational mapper for the Python programming language)
   - oauth2client (handles all steps of the OAuth 2.0 protocol required for making API calls)
   - httplib2 (http client library)

   Apache2 (an open-source HTTP server for modern operating systems including UNIX and Windows)

   Web Server Gateway Interface (WSGI) (simple and universal interface between web servers and web applications or frameworks for the Python programming language)

   3. Summary of configurations made

   a) Proceeded to update all installed packages on baseline Ubuntu server by running the following commands.
    #sudo apt-get update
    #sudo apt-get upgrade

   b) Configured the Uncomplicated Firewall (UFW) to only allow incoming connections for SSH (port 2200), HTTP (port 80), and NTP (port 123).

    #sudo ufw default deny incoming
    #sudo ufw default allow outgoing
    #sudo ufw allow ssh
    #sudo ufw allow 2200/tcp
    #sudo ufw allow www
    #sudo ufw allow ntp
    #sudo ufw allow 123/tcp
    #sudo ufw enable

    Proceed thereafter to change the ssh port from 22 to 2200 by running command 'sudo nano /etc/ssh/sshd_config'. Locate the line "Port 22" and comment it out by adding a # infront of the line. Enter a new line "Port 2200". When this is done, run command "sudo ufw deny 22" to close off the now defunct SSH port 22.

    c) Create new user grader by running the following commands.

    #sudo adduser grader

    d) Proceed to grant sudo access to user grader by executing the following commands. The following file will automatically parse by the system for its content due to the configuration in /etc/sudoers.

    # sudo nano /etc/sudoers.d/grader (copy and paste "grader ALL=(ALL) NOPASSWD:ALL" into file)

    e)  Create key pairs by running command #ssh-keygen in local terminal. 
  
    f) Install above public key in /home/grader/.ssh. Create file .ssh/authorized_keys and paste public key into file.  Further secure the folder by running command "chmod 700 .ssh" to restrict rights and access to folder.

    g) Force key based authentication by changing configurations in /etc/ssh/sshd_config. Run following command, change PasswordAuthentication from yes to no.

      #sudo nano /etc/ssh/sshd_config

    h) Install apache and libapache2-mod-wsgi on Ubuntu server.

    i) Configure Apache to handle requests using the WSGI module by editing the '/etc/apache2/sites-enabled/000-default.conf'  file.

    j) Update '/etc/apache2/sites-enabled/000-default.conf' WSGI file - (i) the ServerName with public IP address given to you by AWS, (ii) DocumentRoot folder which points WSGI to the folder which all other python codes to be executed are stored, (iii) WSGIDaemonProcess - directory where the virtual python environment (i.e. environment which all required Python modules are installed) is installed in (iv) WSGIScriptAlias - the main WSGI file that contains the main code to be executed, (v) Directories that state the locations where all the static (i.e. css) and html templates for the python codes.

        ServerName 54.254.165.218
        DocumentRoot /var/www/html/itemcatalogproject/itemcatalogproject
        WSGIDaemonProcess python-home=/var/www/html/itemcatalogproject/venv/
        WSGIScriptAlias / /var/www/html/itemcatalogproject/myapp.wsgi

        <Directory /var/www/html/itemcatalogproject/itemcatalogproject>
                WSGIProcessGroup itemcatalogproject
                WSGIApplicationGroup %{GLOBAL}
                Order allow,deny
                Allow from all
        </Directory>
        Alias /static /var/www/html/itemcatalogproject/itemcatalogproject/static
        <Directory /var/www/html/itemcatalogproject/itemcatalogproject/static/>
                Order allow,deny
                Allow from all
        </Directory>
        <Directory /var/www/html/itemcatalogproject/itemcatalogproject/templates>
            Order allow,deny
            Allow from all
        </Directory>

      k) Create key application to be executed by WSGI by running the following command  "sudo nano /var/www/html/itemcatalogproject/myapp.wsgi" before pasting the following into the file.

        import sys, os

        sys.path.insert(0, '/var/www/html/itemcatalogproject/itemcatalogproject')

        from coding_core import app as application

        application.secret_key = 'super_secret_key'

4. List of third-party resources

  http://flask.pocoo.org/docs/0.12/deploying/mod_wsgi/
  http://docs.sqlalchemy.org/en/latest/core/engines.html#postgresql
  https://www.digitalocean.com/community/tutorials/how-to-configure-ssh-key-based-authentication-on-a-linux-server
  https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps
  Udacity forums

License/usage restriction

This is a public domain work and there is no license/usage restriction on it. Please feel free to use/modify the codes in anyway to suit your own needs. If you have more questions pertaining to this application, drop an email to Shanfu87@yahoo.com and I will be happy to assist you.
# linuxserverconfiguration
