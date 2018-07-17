# Udacity-Linux-Server-Configuration
This project is for set up a Linux serve and is linked to the Configuring Linux Web Servers course. 

The server is created with Amazon Lightsail for this. 
Get your server.

1. Start a new Ubuntu Linux server instance on Amazon Lightsail. 
	- https://aws.amazon.com/lightsail/
		
2. Follow the instructions provided to SSH into your server. 

# Secure your server.

3. Update all currently installed packages.

	 	sudo apt-get update
	 	sudo apt-get upgrade
	 
4. Change the SSH port from 22 to 2200. Make sure to configure the Lightsail firewall to allow it.

	- Open the configuration file with your favorite text editor.
		
		nano -c /etc/ssh/sshd_config	

	- Line #13 shows the current configured port number. (ie. Port 22) Meaning, SSH is currently using 22.
	- Change the numerical digits from 22 to your desired port number, for this example we will be using 2200.

5. Configure the Uncomplicated Firewall (UFW) to only allow incoming connections for SSH (port 2200), HTTP (port 80), and NTP (port 123).
		
		sudo ufw allow 2200/tcp
		sudo ufw allow 80/tcp
		sudo ufw allow 123/tcp
		sudo ufw enable
   
	 Warning: When changing the SSH port, make sure that the firewall is open for port 2200 first, so that you don't lock yourself out of the server. Review this video for details! When you change the SSH port, the Lightsail instance will no longer be accessible through the web app 'Connect using SSH' button. The button assumes the default port is being used. There are instructions on the same page for connecting from your terminal to the instance. Connect using those instructions and then follow the rest of the steps.

# Give grader access.

In order for your project to be reviewed, the grader needs to be able to log in to your server.

6. Create a new user account named grader.
	
		sudo adduser grader

7. Give grader the permission to sudo.

		sudo vim /etc/sudoers.d/grader
   add text `grader ALL=(ALL) NOPASSWD:ALL`

8. Enable SSH for Grader User
	- Switch to Grader's Home directory and create a new directory called .ssh
	- Run touch .ssh/authorized_keys
	- Open the grader_key.pub and paste them into .ssh/authorized_keys file on VM
	- Change permissions chmod 700 .ssh , chmod 644 .ssh/authorized_keys
	- To make sure key based authentication is forced open /etc/ssh/sshd_config file '# Change to no to disable tunnelled clear text passwords' if the next line is 'PasswordAuthentication yes', change the 'yes' to 'no', save and exit the file, run 
	
		sudo service ssh restart

9. Create an SSH key pair for grader using the ssh-keygen tool. Prepare to deploy your project.
		
		ssh-keygen -t rsa

10. Configure the local timezone to UTC.

		sudo dpkg-reconfigure tzdata
		
11. Install and configure Apache to serve a Python mod_wsgi application.
		
    If you built your project with Python 3, you will need to install the Python 3 mod_wsgi package on your server: 
    
    		sudo apt-get install libapache2-mod-wsgi-py3

    I have followed reference item 1 for setting up the application.
12. Install and configure PostgreSQL:

		sudo apt-get install postgresql postgresql-contrib
	
    Do not allow remote connections
    Create a new database user named catalog that has limited permissions to your catalog application database.

13. Install git.

		sudo apt-get install git

# References
1. https://www.digitalocean.com/community/tutorials/how-to-serve-flask-applications-with-uwsgi-and-nginx-on-ubuntu-16-04
2. https://kiranvm.wordpress.com/2018/07/17/502-bad-gateway-nginxflaskgunicorn-2-no-such-file-or-directory/

