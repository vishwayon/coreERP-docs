Getting the Source
------------------

The source code of coreERP is published on github. Please refer to the `git - Quick Start Guide` to configure git and understand its basic commands.

There is also an excellent article on best practices `A successful git branching model <http://nvie.com/posts/a-successful-git-branching-model/>_`.  

Register on `github <http://github.com>_` and fork the coreERP repo: **vishwayon/coreERP-ce**. 

Clone git repository
~~~~~~~~~~~~~~~~~~~~

Create a new folder in your home directory on your local machine. We name it `gitWorkFolder`. Clone your newly forked github repository into this folder.
Please refer to **git - Quick Start Guide** to familiarise yourself with git concepts and commands.

Clone following repositories:

**vishwayon/coreERP-ce**
**vishwayon/coreERP-vendor**

After having successfully created a clone of the git repository on your local machine, we need to publish the application in apache. 


Publishing in apache
~~~~~~~~~~~~~~~~~~~~

We will first link the vendor files ::

    $ cd ~/gitWorkFolder/coreERP-ce
    $ ln -s ../coreERP-vendor/vendor

This would link all the files required in vendor.

Let us create the symbolic links to the application. This would be inside lampp folder where xampp was installed.

    Create a symbolic link in apache ::

        $ cd /opt/lampp/htdocs/             
        $ sudo ln -s ~/gitWorkFolder/coreERP-ce/web core-erp

    :Note:
        This folder is now published as an application in apache


We need to provide write access to some of the folders. This is where the runtime rendering files are cached. ::

    $ sudo chmod 777 runtime
    $ cd web
    $ sudo chmod 777 assets

Editing the config file
-----------------------

Connection to the database is managed via config file. coreERP has ``sampleconfig.php`` file located in ``cwf/``. 
this file cannot be used directly. We need to create a copy of this file in the root folder of apache where the coreERP site is published. 
This file is in xml format and you can edit it using any text editor.

Creating a copy of sample.xconfig ::

    $ cd ~/gitWorkFolder/coreERP-ce/config/
    $ sudo cp ~/gitWorkFolder/coreERP-ce/cwf/sampleconfig.php cwfconfig.php

.. warning::
	coreERP would read this file **only** if it is in the config folder of the source. This is not the published folder.

Edit the file as follows: ::

    $ sudo nano cwfconfig.php

If you have a development database already setup, enter the credentials of that database.

To setup a new development database, do the following:

    - Set the value for *suPass*. coreERP installation would use the *suName* and *suPass* to create a new login/role in postgreSQL
    - Set the value for *pgPass*. This is the password you had set while configuring *postgres* user according to **Server Setup Guide**
    - Save (ctrl+o), *Enter*, exit (ctrl+x). 

Open a browser and go to the following link: ``http://localhost/core-erp``. See if you get the login screen.



