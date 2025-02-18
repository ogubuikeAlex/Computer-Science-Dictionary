## Console-Based Editor
- VI
- VIM
- Nano, etc

User Account
- whoami //tells who u are
- id //shows user id
- su alex //to switch user to alex
- ssh alex@198.12.12 // When you are accessing one system from another and you want to access using a different user than the host user

- The root user has no restrictions on the system
- Access to root user is restricted usually
- For a normal user to have privileges, they can do this via the "sudo command". Which prompts to drop their password


Download Files From The Internet
- curl URL -O : -O is to specify we want to save file, without it the file will just be printed out
- wget url -O where-to-store.txt

To Know thw current OS
- ls /etc/*release* 
- cat /etc/*release* : to view more

## Package Managers

They help us install various software in systems.

CentOS uses RPM-based package manager like Fedora

RPM -> Red Hat Package Manager.
It bundles a software using the .rpm extension

rpm -i telnet.rpm //install a package
rpm -e telnet //uninstall a package
rpm -q telnet //query if package is installed by rpm. you can do this for multiple packages on one line


It requires you point it to the exact location where the package is available. rpm does not install dependencies the package will use.

This is why we need a solution that can query a package, find the package and install all the package dependencies.
Welcome "yum". It searches repos that have thousand of RPM packages. Finds them and also finds their dependencies and installs them all in the right order.

How does yum find them?
Every OS comes with a list of list of repositories that contain commonly used packages. So you may never need to update this list.
But it is also possible that you will need to configure additional repos for yum to find them. 

-> yum repolist //will show us the list of repositories available in the system

-> ls /etc/yum.repos.d/ //will show the files where the repos are configured

-> cat /etc/yum.repos.d/CentOS-Base.repo //Looking into any of the repo configuration files will show you extra information like the URL where all the packages are stored.

-> yum install repo-rpm-extension-file //this will install a new repo
-> yum list package-name //get installed package
-> yum remove package-name
-> yum --showduplicates list package-name //see all available versions of a package and the repos there are from
-> yum install package-12.1.1 // to install a specific version of a package

Note: you can use "yum install" to install a package directly from a local RPM file, essentially installing a package without using a repository by specifying the full path to the RPM file in the command; just type "yum install /path/to/your/package.rpm" to install a package that is not currently in your configured repositories. 

## Services
Services help configure servers so they run smoothly even when rebooted. Also to make sure the services in the servers and the serves themselves are started in the right order. 

-> service service-name start //Start service

-> systemctl start service-name //Start service

Note: service uses systemctl under the hood. 

-> systemctl stop service-name //Stop service

-> systemctl status service-name //Check HTTPD service status

-> systemctl enable service-name //Configure service to start at startup

-> systemctl disable service-name //Configure service to not start at startup

systemctl  is the command line utility used to manage services in a systemd managed server

What is Systemd?
It is a software suite that manages services and systems for linux operating systems.

What is a systemd managed server?

 What does systemd do?
 - Manages user processes and bootstraps
 - Provides a logging system for viewing log files
 - Mounts devices and manages temporary files
 - creates sockets for networking and manages network time synchronization
 - Manages file system and maintains mount and auto-mount points
- Controls basic system configuration

### How Do We Program A Software As A Service?
To do this we need to configure our app as a systemd service.
This is done via a systemd unit file located at /etc/systemd/system.
The file must be named as what you want the service to be known as with a .service extension

In the file (running.service), assuming its a node app, inside you will declare a service section:
```bash
[Service]
ExecStart=node index.js
```

Then you will notify systemd of a new service by running:
```bash
systemctl deamon-reload
```

After that you can start and stop the service using:
```bash
systemctl start running
systemctl stop running
```

Q: How do we ensure it runs automatically when the system boots up?
This is done in the unit file. This will be done in the install section and the goal is to set the service to run after a particular service that runs at boot up.
The updated file will be:

```bash
[Service]
ExecStart=node index.js

[Install]
WantedBy=multi-user.target
```

After updating the document, we will set it up by running:
```bash
systemctl enable running
```

To disable use:
```bash
systemctl disable running
```

Q: What is WantedBy? What is multi user target run level? Is there a way to order the sections?

We can also add meta data like description to allow others understand what the service is about. this is done under the Unit section:
```bash
[Unit]
Description=My running running running app

[Service]
ExecStart=node index.js

[Install]
WantedBy=multi-user.target
```

- For commands or scripts that should be run before or after starting the application then add the `ExecStatPre` or `ExecStartPost`:
```bash
[Unit]
Description=My running running running app

[Service]
ExecStart=node index.js
ExecStartPre=/opt/code/configure_db.sh
ExecStartPost=/opt/code/email_status.sh

[Install]
WantedBy=multi-user.target
```

Q: What is a real life use case of all these? it feels too abstract!

- If you want the application to automatically restart when it crashes then add Restart:
```bash
[Unit]
Description=My running running running app

[Service]
ExecStart=node index.js
ExecStartPre=/opt/code/configure_db.sh
ExecStartPost=/opt/code/email_status.sh
Restart=always

[Install]
WantedBy=multi-user.target
```

Q: What are other options for restart?
