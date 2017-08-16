Portal Installation
*******************

Overview
========
This code has been tested on Ubuntu v14.04 and later.

Prepare the server
^^^^^^^^^^^^^^^^^^
These are the step that were taken to guarantee a functional portal.

#. Download and install Ubuntu Server 16.04.3 LTS
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
^^^^^^^^^^^^^^^^^^^^^^^^
On an Ubuntu server:

#. Get the code from github:

    ::

        git clone https://github.com/lorist/vmrportal.git

#. Install required modules within the virtual environment:

    ::

        pip install -r ~/vmrportal/requirements.txt


Configure the application
^^^^^^^^^^^^^^^^^^^^^^^^^


Configure NGINX
^^^^^^^^^^^^^^^



Run the application
^^^^^^^^^^^^^^^^^^^
