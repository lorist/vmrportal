Portal Installation
*******************

Overview
========
This code has been tested on Ubuntu v14.04. This application and associated documentation assumes that the Ubuntu user is ``pexip``

Prepare the server
==================
These are the step that were taken to guarantee a functional portal.

#. Download and install Ubuntu Server 14.04 LTS
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

        git clone https://github.com/lorist/vmrportal.git

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

Make application start at boot
==============================

Copy the uWsgi configuration file to correct location:
    ::

        sudo cp ~/vmrportal/portal.conf /etc/init/

The new commands to run the application are:
    * sudo start portal
    * sudo restart portal
    * sudo stop portal

Configure NGINX
===============




Run the application
===================
