Install and Configure CoreERP
=============================

This document details the instructions required to install, configure and run the release version of coreERP on a Ubuntu Server 14.04.
The dependencies required to be installed on the server are listed in **Server Setup Guide**. 
To install coreERP, complete all the steps and tests mentioned in **Server Setup Guide** and then return to this 
document. For developer edition of coreERP, please refer to **Developer's Guide** and setup the development environment.

Installation and setup of coreERP is a simple process, provided all steps mentioned as part of this guide are followed 
thoroughly without skipping any. coreERP is maintained on github. If your intention is to only install and run the application,
a very basic understanding of *git* would suffice. The required commands are covered as part of installation process. If you plan to 
develop or make modifications to the source, a thorough understanding of git is expected. Please refer to the git documentation 
to learn more.

The entire installation process is explained in simple steps along with the commands. Execute the commands on the linux bash shell after 
you have setup the server. All commands and configuration options have been tried and tested on Ubuntu Server 18.04LTS.

Before we start the installation process, please create a github account on http://github.com. 


Getting the Source from *github*
--------------------------------

The release version of coreERP is published on github as `vishwayon/coreERP <https://github.com/vishwayon/coreERP-ce/releases>`_. It may request for authentication information. Please provide the github registered user information. The **Latest release** would be tagged and appears on top of the list. Copy the hyperlink for download **Source code (tar.gz)** (usually a right click and 'copy link address').

We start by downloading the release version on the server. This is preferably created in /opt ::

    $ cd /opt
    $ sudo curl -u <github-username> -L -o coreERP-release.tar.gz "<paste link to download>"
    $ sudo tar zxvf coreERP-release.tar.gz
    $ sudo mv coreERP-<ver.no> coreERP

Alternatively, you may clone the git repo (use the master branch for stable release version) ::

    $ sudo git clone https://github.com/vishwayon/coreERP-ce.git
    $ sudo git clone https://github.com/vishwayon/coreERP-vendor.git


Publishing in apache
--------------------

CoreERP has seperate Application shell and modules. To relate the two, we create symbolic links to the application and links to apache.

Create a symbolic link in apache and publish only the **web** folder ::

    $ cd /var/www/html
    $ sudo ln -s /opt/coreERP-ce/web/ core-erp

:Note:
    This folder is now published as an application in apache

Create a symbolic link between CoreWebApp and CoreWebBase/core ::

    $ cd /opt/coreERP-ce
    $ sudo ln -s ../coreERP-vendor/vendor/ vendor

    :Note:
        The source core is now available to the web application. If required, you can proceed to create multiple linked folders
        in source to publish more modules.

We need to provide write access to some of the folders in CoreWebApp. This is where the runtime rendering files are cached. ::

    $ sudo chmod 777 runtime
    $ cd web
    $ sudo chmod 777 assets
    $ sudo rm reportcache
    $ sudo ln -s /opt/tomcat/webapps/reportcache reportcache

Creating symbolic links and publishing the application has the advantage of storing all code in one location. When we update the source, it would
automatically reflect everywhere.

Publishing Report Server on Tomcat
----------------------------------

coreERP uses the pentaho reporting engine to render all reports. The tomcat installation that was downloaded and installed in **coreERP - Server Setup Guide**
contains the required libraries of pentaho.

    - Download the `CoreJSReportServer.war <https://storage.googleapis.com/server-setup/CoreJSReportServer.war>`_ file on your local node (not on the server).

    - ssh to the server "-l 8080:localhost:8080" and open a tunnel. Using a browser, open the following link to your server.
		http://localhost:8080 	# This should display the tomcat welcome screen.

        - click on ``Manager App``. It would be a button on the top right hand side of the tomcat welcome screen
        - It would prompt you for authentication. Use the following
			- user name: [Find username from the tomcat config]
			- password: [Find password from the tomcat config]
        - It will list all the applications deployed on the server. Scroll to the `Deploy` section -> `WAR file to deploy`
        - Click on ``Choose File`` and select the *CoreReportServer.war* file that you previously downloaded.
        - Click on ``Deploy``. It would automatically upload the file and publish it as an application on the server.


:Tests:
    You can then click on the published application ``/CoreJSReportServer`` and see if it displays the following
		
		``Congratulations! Report server is deployed and working on Tomcat!``

.. note:: 
    If you want to harden the report server, you can disable bindings in tomcat and restrict the listening ports
    to localhost only. This would ensure that tomcat is no longer available to the outside world. This is a recommended setting
    as coreERP requires tomcat to listen only on localhost. Else, you can disable port 8080 on the firewall. You may also change the authentication
    information in tomcat. Please refer to the tomcat documentation for further details.

Editing the config file
-----------------------

Connection to the database and other configuration settings are managed in a config file. coreERP has ``sampleconfig.php`` file located in ``CoreWebBase\cwf``. 
this file cannot be used directly. We need to create a copy of this file in the config folder of CoreWebApp where the coreERP source is published. 
This file returns a php array and you can edit it using any text editor.

Creating a copy of sampleconfig.php ::

    $ cd /opt/coreERP/CoreWebApp/config
    $ sudo cp /opt/coreERP/CoreWebBase/cwf/sampleconfig.php cwfconfig.php

.. warning::
	coreERP would read this file **only** if it is in the config folder of the coreERP source.

Preview of the file

.. code-block:: php

	return [
        'dbInfo' => [
            'dbServer' => '127.0.0.1',
            'dbMain' => 'main',
            'suName' => 'coreadmin',
            'suPass' => 'password'
        ],
        'pgInfo' => [
            'pgUser' => 'postgres',
            'pgPass' => 'password'
        ],
        'rootModules' => [
            'core' => 'app\core\CoreModule'
        ],
        'dbBackup' => [
            'compress' => 'singleFile',
            'path' => '/path/to/dbbackup/'
        ],
        'mailer' => [
            'host' => 'smtp.mydomain.com',
            'username' => 'sender@mydomain.com',
            'password' => 'pasword',
            'port' => '587'
        ],
        'restrictIP' => true,
        'exceptionMail' => [
            'to' => 'support@mydomain.com',
            'from' => 'noreply@mydomain.com'
        ],
        'dm' => [
            'path' => '/opt/filestore/'
        ],
    ];


Edit the file as follows (or you may use your favourite editor to edit this file): ::

    $ sudo nano cwfconfig.php

- Set the value for *suPass*. coreERP installation would use the *suName* and *suPass* to create a new login/role in postgreSQL
- Set the value for *pgPass*. This is the password you had set while configuring *postgres* user according to **Server Setup Guide**
- Set tge mailer config and the exception mail config.
- Save (ctrl+o), *Enter*, exit (ctrl+x). 

Setting up the Database
-----------------------

Creating the initial database is a console process. All the required database scripts are included as part of the source. Each module in the source
contains its corresponding script file. The database objects are created based on these script files.

Let us now call the shell command to create and initialise the *main* database. :: 

    $ cd /opt/coreERP-ce
    $ php yii installer/install/start
		
If the config file has been copied and everything is correct, it should start creating the user and the database.

The output on the shell should give messages of successfully completed steps. If there are any errors encountered during execution, please read the errors and fix the issues.
Most of the common errors include *postgres database already exists* or *postgres user already exists*. You can either drop the objects from postgres or modify the config 
file to provide new names. 


Verify successful installation
------------------------------

If everything went off successfully, you should be able to open coreERP via the following link on your node: 

	``http://<remote-ip>/core-erp``

It should load and display the login screen. Login using the suName and suPass that you have mentioned in your config file. 
You can now proceed to the next topic on **Getting Started** or refer to **Component Setup** for installing other optional components.





	


		
	
	 
	
