---
title: Setting up multiple virtual hosts
date: 2010-08-29 11:33:00 +05:30
categories:
- miscellaneous
tags:
- apache
- server
author: Smit Shah
---

Few days ago I had face a problem,"How to use different domains on localhost?"so at that time i doesn't know how to use different/multiple domain on localhost so i just put project on Development server and then i check the functionality.

Today I got some time to find out "How to make multiple domain on localhost"
so i got solution as always :)


The multiple hostnames such as projecta, projectb, projectc and projectc-beta2 can be set in either of these hosts configuration files (also referred to as hosts files) depending on your operating system:


`Windows: C:\WINDOWS\SYSTEM32\DRIVERS\ETC\HOSTS (assuming your Windows installation is in C:\WINDOWS). 
For Linux: /etc/hosts 
If you were to open these files in a text editor, you’ll see content similar to the following: 
127.0.0.1        localhost 
192.168.1.18     testhost.testdomain.com testhost`


All hosts files will have the localhost entry which resolves to 127.0.0.1, also known as the loopback address. Basically, what the first line in the host file illustrated above tells us is to route all network packets addressed to localhost to the computer you’re using.
So to get our fake hostnames working, they each should have an entry resolving to 127.0.0.1 in your hosts file. Example as follows:


`127.0.0.1        localhost
127.0.0.1        projecta
127.0.0.1        projectb
127.0.0.1        projectc
127.0.0.1        projectc-beta2
192.168.1.18     testhost.testdomain.com testhost`


To test the configuration, you should ping the fake hosts and if you had followed the steps above correctly, you should get a response from 127.0.0.1 for the projecta, projectb, projectcand projectc-beta2 hosts. This concludes the host resolving section of this tutorial.
Configuring Apache


The next step involves configuring Apache to handle requests to the hosts defined above. To do this, you’ll need to edit your Apache installation’s configuration file; httpd.conf. On a default Apache for Windows installation, this file will be in C:\Program Files\Apache Group\Apache\conf. For Linux, the location of the httpd.conf file usually depends on the distribution. On Slackware Linux, it’s in /etc/apache/.


Open the httpd.conf file in your favourite text editor and look for a line that starts with DocumentRoot. This line sets the location for the files that will be served as your default web site.


On Linux, it will look something like the following (Note that the actual location may differ on your setup):
DocumentRoot "/var/www/htdocs"
Whereas on Windows, it will be similar to (Note that the actual location may differ on your setup):
DocumentRoot "C:/Program Files/Apache Group/Apache2/htdocs"
Also note that for Windows, Apache can also use a normal slash (/) instead of the typical backslash () in the directory path. You should also enclose directory paths in quotes. From now on, we’ll refer to these directories as DocumentRoot.


If the directories where you want to serve content for the hosts created earlier reside within the DocumentRoot directory, you merely need to define a VirtualHost declaration for each of them. Let’s assume that the files for http://projecta will be in the directory named projecta under the DocumentRoot directory (which will be /var/www/htdocs/projecta/for Linux or C:\Program Files\Apache Group\Apache2\htdocs\projecta\ for Windows).
Therefore, at the end of your httpd.conf file, append the following lines to create the projecta virtual host:


For Linux:

`DocumentRoot /var/www/htdocs/projecta
ServerName slackbox`

For Windows:

`DocumentRoot "C:/Program Files/Apache Group/Apache/htdocs/projecta"
ServerName slackbox`


If the files are served from a directory outside of the DocumentRoot, you need to create a Directory declaration for each of the directories in addition to the VirtualHost declaration.

For example, let’s assume that files from projectb will be served from /web/projectb (for Linux) or C:\web\projectb (for Windows). Therefore, the following lines should be appended to your httpd.conf file:


For Linux:

`Options Indexes FollowSymLinks MultiViews
AllowOverride All
Order allow,deny
Allow from all
DocumentRoot /web/projectb
ServerName projectb`

For Windows:

`Options Indexes FollowSymLinks MultiViews
AllowOverride All
Order allow,deny
Allow from all
DocumentRoot "C:\web\projectb"
ServerName projectb`



Testing the Setup
Once all of the above is done, you can now test your configuration. However, you need to restart the Apache service in order to apply the changes made above. Here’s how to do it if you’ve installed Apache as a service (9 times out of time, this is how it’s normally installed):


For Linux (distros using BSD-style init like Slackware):
`/etc/rc.d/rc.httpd restart`


For Linux (distros using sysv-style init like Red Hat):
`service httpd restart`


For Windows:

Run: services.msc
Look for Apache in the services list and select it
Click the Restart Service button


If you’ve followed through this guide closely, you should now have a working Apache installation with multiple named local sites.