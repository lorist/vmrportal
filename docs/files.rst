Portal Files
************************
This section describes the files that require changing to get the portal up and running.


Config
======
The main configuration file is the ``config.py`` file. This file contains the configuration parameters that the main
application uses.

config.py::

          ######################################
          ####### Pexip myVMR config file ######
          ######################################

          ####### Active Directory details:
          LDAP_HOST = 'ad01.customer.com'
          LDAP_BASE_DN = 'CN=Users,DC=custom,DC=com'
          LDAP_USERNAME = 'pexservice@customer.com'
          LDAP_PASSWORD = 'yourpassword'
          LDAP_USER_OBJECT_FILTER = '(&(objectCategory=person)(objectClass=user) (sAMAccountName=%s))'

          ####### Pexip Managament node details:
          MGR_ADDRESS = 'mgr.customer.com'
          MGR_USER = 'admin'
          MGR_PASSWORD = 'yourpassword'

          ####### VMR details:
          CONTROL_YOUR_VMR_FQDN = 'meet.customer.com' #FQDN or IP of conference node or RP for webRTC. The 'Control your VMR' link
          WEBRTC_FQDN = 'meet.customer.com' #FQDN or IP of conference node or RP for webRTC. Appears in webRTC links and 'Copy to clipboard'
          PHONE_VR = '+6183100000'

          ####### User Theme permissions (future):
          ALLOW_THEMES = [1,2,3,4] # IDs of pin themes allowed to be set by users for thier VMRs

          ####### Display Scheduled and Devices tabs:
          SCHEDULING = 'true' #set to 'true' if you have Pexip scheduling enabled and want to experiment. Note that this feature is not tested beyond a small number of scheduled meetings
          DEVICES = 'true' # set to true if you have devices enabled for users

          ###### Log file name:
          LOG_FILENAME = 'portal.log'


LDAP configuration
==================
This section describes the parameters required for the portal to communicate with an LDAP/Active Directory server for user authentication.

* ``LDAP_HOST`` is the IP or FQDN of your LDAP or Active Directory server.
* ``LDAP_BASE_DN`` is the base search DN, eg: 'OU=All Staff,DC=customer,DC=com'.
* ``LDAP_USERNAME`` is the bind username used by the application to connect to LDAP 'ldapsearch@customer.com' or 'CUSTOMER\\ldapseach'.
* ``LDAP_PASSWORD`` is the password associated with the above user.
* ``LDAP_USER_OBJECT_FILTER`` is the object filter to identify the user logging into the portal. Most often this will be the ``sAMAccountName`` '(&(objectCategory=person)(objectClass=user) (sAMAccountName=%s))'

Pexip Management
================
* ``MGR_ADDRESS`` is the IP address or FQDN of the Pexip Management node.
* ``MGR_USER`` is the username that the portal wil use to talk to the Pexip Management node. You can use the admin account or an LDAP account with API role.
* ``MGR_PASSWORD`` is the password to the above user account.

VMR details
===========
* ``CONTROL_YOUR_VMR_FQDN`` 'vc.pexip.com.au' #FQDN or IP of conference node or RP for webRTC. The 'Control your VMR' link
* ``WEBRTC_FQDN``  'vc.pexip.com.au' #FQDN or IP of conference node or RP for webRTC. Appears in webRTC links and 'Copy to clipboard'
* ``PHONE_VR`` is the telephone number that is pre-appended to the phone dial-in details

User Theme permissions
======================

.. ToDo::  Allow users to select the pin theme to use. Handy for turning on and off the entry and exit tones for instance.

Menu Headings
=============
* ``SCHEDULING`` set to 'true' if you have Pexip scheduling enabled and want to experiment. Note that this feature is not tested beyond a small number of scheduled meetings
* ``DEVICES`` set to true if you have devices enabled for users

Logging
=======

* ``LOG_FILENAME`` logfile name

HTML
====

The HTML files for the project are located in the /templates directory

Inherited layout
================
The basic layout of the portal is defined in the the ``layout.html`` file. This file has the code that defines the look
and feel of all pages. This includes the nav bar at the top of the page.

At the beginning of all other HTML files the layout file is called using:
 ``{% extends "layout.html" %}``
