# Linux Server Configuration

This project presents a basic linux instance configured to serve a simple web application.

* You can find the server at: 18.220.220.2
* SSH is enabled there at: port 2200
* The application is located at the server's root: http://18.220.220.2/

In order to complete this project, I began with an Amazon Web Services Lightsail instance. 

## Security

Right away, I updated all software packages using `sudo apt-get update` followed by `sudo apt-get upgrade`. Orphaned packages were then removed using `sudo apt autoremove`.

To improve networking security on the server, I then configured the Uncomplicated Firewall (UFW) to block traffic to all ports except 20, 80, 123, and 2200, while leaving all outgoing ports open. Next, I created SSH key-pairs for a `grader` user via ssh-keygen on my local machine. Back on the remote machine, I created a new user named 'grader', and granted it sudoer privileges. I then entered the public key into 'authorized_keys' file in the user's .ssh directory to enable SSH login as grader. With that step complete, I could now access my Lightsail instance without the AWS console, instead logging in with the 'grader' private ssh key. This means I could deny traffic to port 20.

Finally, I disabled password-based logins.

## Configuring the Application

As grader, I then installed all the software necessary to run the web application. Since Lightsail already came with Python, I began by installing Apache2, then WSGI. Next, I cloned the repository of my Catalog project to a subdirectory in the `/var/www` directory. I then set the apache2 configuration to serve my Catalog project as a WSGI application.

Next, I installed all dependencies for the Catalog project (first installing pip3). This included Flask (and all its dependencies), postgresql and several others. I then made the necessary modifications to my source code to migrate the project from using sql-lite to postgresql. This required creating a postgresql user (also named grader, and with an eponymous password), and creating the initial database via commandline in the psql shell. 

To enable OAuth2 login for the application, I generated a new client_secrets file and refactored the python code to point to this file using absolute paths. 

## Epilogue

This summary skips over the many hours of frustration and pointless changes back and forth between settings that probably never needed to be changed in the first place. This included much uninstalling and installing python modules and pointing my python-path in the WSGIProcessDaemon all over the place. It is entirely likely that I've failed to mention relevant changes, but overall, I believe the above summary reflects the things I did that actually mattered. In addition to guidance from Forum Mentor Trish, I read documentation from all over and enjoyed the collective wisdom of Stack Overflow questions.

