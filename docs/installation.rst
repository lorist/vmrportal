Portal Installation
*******************

Overview
========
This code has been tested on Ubuntu v14.04. This application and associated documentation assumes that the Ubuntu user is ``pexip``

Prepare the server
==================
These are the step that were taken to guarantee a functional portal.

#. Download and install Ubuntu Server 14.04 LTS (be sure to enable OpenSSHServer)
#. Connect to the machine via SSH
#. Update the operating system

    ::

        sudo apt-get update && sudo apt-get upgrade

#. Install the required packages

    ::

        sudo apt-get install python-pip python-dev nginx git libsasl2-dev libldap2-dev libssl-dev
        sudo pip install virtualenv

#. Create the Portal's virtual environment:

    ::

        cd ~/ && virtualenv vmrportalvenv

#. Activate the virtual environment:

    ::

        source ~/vmrportalvenv/bin/activate


Download the application
========================
On an Ubuntu server:

#. Get the code from github:

    ::

        cd ~/ && git clone https://github.com/lorist/vmrportal.git

#. Install required modules within the virtual environment:

    ::

        pip install -r ~/vmrportal/requirements.txt


Configure the application
=========================

Edit the ``~/vmrportal/config.py`` file to suit your environment. See: :doc:`files` for more details.

Test the application
====================
To check that your configuration is working correctly and that the application can communicate with LDAP, it is possible to run
the application in isolation, without configuring it behind Nginx.

#. To run the application in debug mode:
    ::

        cd ~/vmrportal && python portal.py

    If the application is working correctly, you will see the following lines:

    * Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)
    * Restarting with stat
    * Debugger is active!
    * Debugger pin code: 530-789-712

#. Browse to the portal: http://<server_ip>:5000/portal

#. To exit, <CTRL-C>

Make application start at boot
==============================

Copy the uWsgi configuration file to correct location:
    ::

        sudo cp ~/vmrportal/portal.conf /etc/init/



Configure NGINX
===============

Edit the Nginx default configuration:

    ::

        sudo rm /etc/nginx/sited-enabled/default
        sudo nano /etc/nginx/sites-enabled/default


Paste in the following config:

    ::

        # HTTPS server
        #
        server {
                listen 443;
                server_name localhost;

                root html;
                index index.html index.htm;

                ssl on;
                ssl_certificate ssl/cert.pem;
                ssl_certificate_key ssl/cert.key;

                ssl_session_timeout 5m;

                ssl_protocols SSLv3 TLSv1 TLSv1.1 TLSv1.2;
                ssl_ciphers "HIGH:!aNULL:!MD5 or HIGH:!aNULL:!MD5:!3DES";
                ssl_prefer_server_ciphers on;

                location / {
                    return 301 /portal;
                    }
                location /static {
                    alias /home/pexip/vmrportal/static;
                    }
                location /portal { try_files $uri @yourapplication; }

                location @yourapplication {
                    include uwsgi_params;
                    uwsgi_pass unix:/home/pexip/vmrportal/portal.sock;
                    access_log /var/log/nginx/portal.access.log;
                    error_log /var/log/nginx/portal.error.log;
                }
        }

Save the file (``CTRL+X`` then ``Y``)

HTTPS Certificate
^^^^^^^^^^^^^^^^^

#. Create the folder:

    ::

        sudo mkdir /etc/nginx/ssl

#. Create a self signed certificate to test with. Note you will want to create and apply a proper certificate and key for production.

    ::

        sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/nginx/ssl/cert.key -out /etc/nginx/ssl/cert.pem

#. Restart Nginx:

    ::

        sudo service nginx restart
        

Run the application
===================

The new commands to run the application are:
    * sudo start portal
    * sudo restart portal
    * sudo stop portal

Start the application:

    ::

        sudo start portal

Start Nginx:

    ::

        sudo service nginx restart

Browse to the IP address of your Ubuntu machine.

You should now be able to login to the portal.
