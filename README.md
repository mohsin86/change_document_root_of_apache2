# change DocumentRoot of apache2 to new directory
Problem: How To Move an Apache Web Root to a New Location on Debian or change DocumentRoot of apache2 to new directory

Prerequisite: 
	1. Needed a mountded volume
	 	if you have mounted volume, you can skip this step, if Not carray on... 
	        		a. if you haven't mounted the volume, first mount the volume, 
                     you can mount the volume by command:  f "sudo mount -t auto /dev/sda10 /path/to/mount/point " or “sudo gnome-disks” via graphical interface. My advice would be to create mount point graphically. You can set mount auto at start up.
			Example: I have mount the volume to 
					sudo mount -t auto /dev/sda10  /mnt/office	
      		 	b. Create a folder under  /mnt/office	
                                               sudo mkdir -p /mnt/office/www
       
	2. Need ownership and permission of directory
		   the comman	that change the directory and file onwerships, and permissions: 
	 	   
		#  sudo chown -R xyz /mnt/office/www 
		# sudo chown -R xyz /mnt/office  
		# sudo chmod 755 /mnt/office/www
		# sudo chmod 644 /mnt/office/www/*
			Here xyz are user
now we want www directory of office mount point as a document root of apache

Now main step to follow:
	1. edit apache configuration file as follow (/etc/apache2/apache2.conf)
                             open  apache2.conf  add
				<Directory /mnt/office/www/>
            				Options Indexes FollowSymLinks
           					 AllowOverride None
            				Require all granted
				</Directory>  
                     see image: edit-apche2.conf.png

2. then go to /etc/apache2/sites-enabled, here open the file 000-default.conf, change DocumentRoot to /mnt/office/www
   see image 000-default.conf.png
			

3. Then, go to /etc/apache2/sites-available, open 000-default.conf, change DocumentRoot to /mnt/office/www , see image sites-available 000-default.conf.png
		
Once you’ve finished the configuration changes, ensure the syntax is correct with the following command:
		sudo apachectl configtest

if output like the following:
	
As long as you see Syntax OK, restart the web server. Otherwise, track down and fix the problems it reported. 

Use the following command to restart Apache:
    sudo service apache2 restart
