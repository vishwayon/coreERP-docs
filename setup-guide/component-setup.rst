Setup Utilities and Components
==============================

The following components are optional. It is recommended that you install all these components to achieve satisfactory performance of the system. 

Install documentation and help
------------------------------

Documentation is a part of the project and is written in reStructured text. This is published via `sphinx-doc <http://sphinx-doc.org>`_.

Please open a ssh terminal to the server and execute the following commands to install `pip` and `sphinx-doc`. ::
   
    $ sudo apt-get install python-pip
    $ sudo pip install Sphinx

Compile the source documentation to html. First create a folder for html output. ::

    $ cd /opt/coreERP     #Or you can select any target folder
    $ sudo mkdir coredocs

Move to the source directory and build output. ::

    $ cd /opt/coreERP/docs
    $ sphinx-build -b html . /opt/coreERP/coredocs/

Now create a linked folder in apache and publish the documentation. ::

    $ cd /var/www/html
    $ sudo ln -s /opt/coreERP/coredocs docs

:Tests:
    
    Open a browser from your local node and browse at ``http://<remote-ip>/docs``

Install Mail Sender Utility
---------------------------

All automated mails in coreERP are dispatched by the *Mail Sender* utility. This utility is installed as an Ubuntu Service that starts automatically 
upon machine start. 

In *Ubuntu 16.04 LTS* or higher this is handled by a utility called *Systemd*.

Open a ssh terminal to the server and type the following ::

    $ sudo nano /etc/systemd/system/coreERP-mailer.service

Copy the following code into the .conf file with changes to /path/to/coreERP (ensure that this reflects the proper path that you used for download of source)

::

	[Unit]
	Description=CoreERP auto mailer service

	[Service]
	ExecStart=/usr/bin/php /path/to/coreERP/yii mailer/mail/sendmail
	User=root
	Restart=always
	RestartSec=30


	[Install]
	WantedBy=multi-user.target


Remember to change the /path/to/coreERP to reflect the proper path of 
coreERP installed on the local drive. Save and close (ctrl+o), (ctrl+x).

This script would ensure that everytime your server is restarted, the script would automatically start. 

To ensure that it starts automatically with every reboot ::

    $ sudo systemctl enable coreERP-mailer
    $ sudo systemctl start coreERP-mailer

To verify the status, type ::

    $ sudo service coreERP-mailer status

If you run into any problems, look into the log file situated in /var/log/syslog

Autostart Tomcat for Report Server
----------------------------------

Using *systemd*, we will create one more script to automatically start tomcat, required by core-ERP for reporting services.

Open a ssh terminal to the server and type the following ::

    $ sudo nano /etc/systemd/system/coreERP-ReportServer.service

Copy the following code into the file with changes to /path/to/tomcat (ensure that this reflects the proper path that you used for download of source)

::

    [Unit]
    Description=CoreERP Report Server on Tomcat8

    [Service]
    Type=oneshot
    RemainAfterExit=true
    ExecStart=/path/to/tomcat8/bin/startup.sh
    User=root

    [Install]
    WantedBy=multi-user.target

To ensure that it starts automatically with every reboot ::

    $ sudo systemctl enable coreERP-ReportServer
    $ sudo systemctl start coreERP-ReportServer

To verify the status, type ::

    $ sudo systemctl status coreERP-ReportServer

If you run into any problems, look into the log file situated in /var/log/syslog

Install Backup Utility
----------------------

All CoreERP data is stored in postgreSQL databases. It is very important to take backup of these databases and store them securely. 
In case of any data loss caused due to a hardware failure or for reasons unknown, the database backup is the only assured mode of retrieving data with minimum loss.

CoreERP has a console based utility that can be setup to automate the tasks of database backup. This utility can be scheduled in *systemd*. 

In ubuntu 16.04 and above, we can use systemd to time these services. 
A good article on this topic is available at https://www.certdepot.net/rhel7-use-systemd-timers/

Create a file coreERP-dbbackup.service in folder /etc/systemd/system/ and copy the following code ::

    [Unit]
    Description=coreERP-dbbackup

    [Service]
    Type=oneshot
    ExecStart=/usr/bin/php /opt/coreERP/yii utils/dbutil/backup
    User=root

    [Install]
    WantedBy=multi-user.target

Let's associate this with a timer. Create one more file coreERP-dbbackup.timer in the same folder and copy the following ::

    [Unit]
    Description=coreERP-dbbackup twice daily

    [Timer]
    # every morning 8am and night 10pm
    OnCalendar=*-*-* 08,22:00:00
    Persistent=True
    Unit=coreERP-dbbackup.service

    [Install]
    WantedBy=timers.target

Now activate the services ::

    $ sudo systemctl enable coreERP-dbbackup.service
    $ sudo systemctl enable coreERP-dbbackup.timer
    $ sudo systemctl daemon-reload

    $ sudo systemctl start coreERP-dbbackup.timer


Backup Settings in cwfconfig
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Your cwfconfig.php script should include the following items: ::

    'dbBackup' => [
        'compress' => 'singleFile',
        'path' => '/path/to/dbbackup/'
    ],

The compress attribute accepts one of the following options:
    
    - **none** - each database backup would be stored as sql file without any compression in a subfolder named by the database.
    - **singleFile** - all backups would be compressed (using Gzip) into a single *tar.gz* file within the backup folder
    - **multiFile** - each database backup would be compressed seperately using Gzip and stored in a subfolder named by the database.  

Modify the **/path/to/** a folder on the server with appropriate access previleges and sufficient space for storage. 
This should preferably be a seperate drive mounted on the server or a network path via nfs. 

CoreERP Session Management
--------------------------

Session management and cleanup of cached user files can be managed via a service in *systemd*. 

Open a ssh terminal to the server and type the following ::

    $ sudo nano /etc/systemd/system/coreERP-session-manager.service

Copy the following code into the file with changes to /path/to/ (ensure that this reflects the proper path that you used for coreERP)

::

    [Unit]
    Description=CoreERP session manager service

    [Service]
    ExecStart=/usr/bin/php /path/to/coreERP/yii utils/session/killexpired
    User=root
    Restart=always
    RestartSec=300
    
    
    [Install]
    WantedBy=multi-user.target

To ensure that it starts automatically with every reboot ::

    $ sudo systemctl enable coreERP-session-manager
    $ sudo systemctl start coreERP-session-manager

To verify the status, type ::

    $ sudo systemctl status coreERP-session-manager

If you run into any problems, look into the log file situated in /var/log/syslog


:Note: If you have installed CoreERP in a virtual path, then mention the appropriate source path that leads to *CoreWebApp*.

The default setting is to invalidate a session without activity for 20 minutes. All cached files are also removed upon session logout by this service. 
The service requires access to the files and folders on the server inside web/reportcache. Hence, it would run under higher previlege account. 

Install ClamAV (Antivirus)
--------------------------

Linux system are not usually prone to viruses. However, any system that allows for file uploads could possibly act as 
a intermediary to transfer infected files from one user to another. It is therefore recommended to use a Antivirus 
package on the server.

We recommend using ClamAV as it is Open Source and under active development. It does not repair infected files and does not
conduct realtime file scans. Hence, there is minimum overhead on the server. ::

    $ sudo apt-get install clamav clamav-daemon
    $ sudo freshclam                            -- This will get the latest virus definitions
    $ sudo service clamav-daemon start          -- This will start the daemon

In the cwfconfig of the application, you can enable fileAVScan ::

    'fileAVScan' => [
        'class' => 'app\cwf\vsla\utils\ClamScan',
        'tmpPath' => '/opt/scan/'  //Ensure that daemon or www-data has access to this folder. also include user: clamav in the daemon/www-data group 
    ],

 
Reset admin password
--------------------

To reset the admin password, run the following command  ::

	php yii installer/install/changesu
	
This command will change the admin password to the one mentioned in the cwfconfig file.
  

ModSecurity Setup
-----------------

To install modsecurity

    $ sudo apt-get install modsecurity-crs


To enable modsecurity, go to the server configuration file. The default location is 

        /etc/apache2/sites-available/000-default.conf


Open the file in editor and add the below line in the <VirtualHost> tag.

        SecRuleEngine On


For REST compatibility, PUT and DELETE methods are required. For this setup, open file

        /etc/modsecurity/crs/crs-setup.conf


Find the parameter for HTTP Methods. Then add PUT DELETE and uncomment the section.

        SecAction \
            "id:900200,\
             phase:1,\
             nolog,\
             pass,\
             t:none,\
             setvar:'tx.allowed_methods=GET HEAD POST PUT DELETE'"


Reload Apache to activate ModSecurity

    $ sudo systemctl reload apache2

