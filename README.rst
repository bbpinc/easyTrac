==================
easyTrac installer
==================

The easyTrac installer is a source-installation kit that installs a complete Trac environment and all its dependecies.

Feedback/bugs to:
 * Email: manuel.viera.tirado at gmail.com
 * Blog: http://blog.manuelviera.es
 * Twitter: http://twitter.com/mviera


Installation instructions
=========================

The installer will compile Nginx and uWSGI and it will install all required python dependencies also.
But for compile Nginx and uWSGI a few dependencies are required to be installed in the system. This dependencies 
are the following:

Dependencies
------------
 * git
 * python-dev
 * gcc
 * make
 * libpcre3-dev
 * libssl-dev 
 * libxml2-dev
 * libxslt-dev
 * libsqlite3-dev
 * libzzip-dev
 * libapr1-dev
 * libaprutil1-dev
 
PLEASE NOTE: You can run the easyTrac installation as root or as a normal user.

The installer config works out of the box, but if you want, you can edit buildout.cfg and 
modify the following parameters:
 * nginx-http-port: http port that will be used for Nginx.
 * nginx-https-port: https port that will be used for Nginx (in case you want to use https).
 * supervisor-http-port: http port that will be used for supervisor.
 * host: host ip address of fqdn.
 * socket-directory: directory where the socket will be stored.
 * pid-directory: directory where the pid files will be stored.
 * log-directory: logs directory.
 * trac-projects-directory: directory where the Trac projects will be created. This directory is <installdir>/opt/trac/, by default.
 * svn-repository-directory: directory where the svn repositories will be created. This directory is <installdir>/opt/svn/, by default.

Once you have installed all required dependencies, you can continue with the installation process.
So you execute the following command:

$ python bootstrap.py


You should have the buildout environment ready to go. Now, you must execute the buildout installation:

$ ./bin/buildout


NOTE: Also, easyTrac compiles Subversion and installs the subversion bindings needed for Trac to manage and browse through svn code repositories.


How to start, stop, restart services
====================================
Supervisor provides a control script to start, stop or restart our 
services. In our supervisor we only get two services: nginx and uwsgi.

The supervisorctl syntax is:

$ ./bin/supervisorctl <command> <service-name>

start services
--------------

$ ./bin/supervisorctl start nginx

stop services
-------------

$ ./bin/supervisorctl stop nginx

restart services
----------------

$ ./bin/supervisorctl restart nginx

Also, supervisor provides an especial word 'all', so we don't need to specify 
every service name. For example, if we need to restart all services:

$ ./bin/supervisorctl restart all


Supervisor has an admin panel on where we can control our services via web. This admin control 
panel is accesible at:

http://localhost:9000

NOTE: by default, the username is 'admin' and the password is 'admin.' To change the admin password, 
edit the [supervisor] part in buildout.cfg and run bin/buildout.


How to create a Trac project
============================

$ ./bin/trac-admin opt/trac/demo initenv demo sqlite:db/trac.db


How to create a svn code repository
===================================

$ ./bin/svnadmin create opt/svn/demo


Uninstall instructions
======================

1) Stop supervisord by running: ./bin/supervisorctl shutdown
2) Remove the folder <installdir>/easyTrac


Backup instructions
===================

easyTrac includes a backup script. So, if you want to make a backup you only must to 
execute the following command:

$ ./bin/backup

and all the trac projects will be backed up into a tarball to the backups directory, called 'backups'.


Restore instructions
====================

Also, easyTrac provides a restore script, useful to restore old backups. Its usage is as simple as follow:

$ ./bin/restore backups/backup-file.tar.gz

and all the trac projects will be restored.
