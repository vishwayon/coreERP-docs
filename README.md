# coreERP-docs

Install documentation and help
------------------------------

Documentation is a part of the project and is written in reStructured text. This is published via `sphinx-doc <http://sphinx-doc.org>`_.

Please open a ssh terminal to the server and execute the following commands to install `pip` and `sphinx-doc`. ::
   
    $ sudo apt-get install python3-sphinx

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
