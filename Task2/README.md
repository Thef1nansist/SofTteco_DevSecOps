# Windows
<br>
## Task: <br>
Set up two servers 1 Django 1 Postgres that communicate with each other (Django need a SQL server). The communication between them should use FQDN (and be encrypted). In Virtualbox. You may use Vagrant boxes or install software with ansible on an empty RHEL 8.<br>
 
 [-Info about Ubuntu 20.04](https://releases.ubuntu.com/20.04/) <br>
 [-Info about VirtualBox](https://www.virtualbox.org/)<br>
 [-Info about VagrantBox](https://www.vagrantup.com/)<br>
 [-Info about PostgreSql](https://www.postgresql.org/)<br>
 [-Info about Django](https://www.djangoproject.com/)<br>
 [-Info about Virtualenv](https://virtualenv.pypa.io/en/latest/)<br>
 [-Info about WSL2](https://winitpro.ru/index.php/2020/07/13/zapusk-linux-v-windows-wsl-2/)<br>
 
 ## Objective of the project: <br>
 The goal of the project is to gain more experience with  VagrantBox, VirtualBox, Django, PostgreSQL, as well as the network, all this is implemented on Ubuntu  <br>
 
 ## Requirements:
 1. Windows 10 - version +19042.928
 2. VirtualBox - version +6.1.22 (Windows version)
 3. WSL 2
 4. Vagrant +2.2.18 (Linux version)
 5. Vagrant plugin: virtualbox_WSL2
 6. Linux Ubuntu 20.04(for vagrant box)
 7. Python (3.8.10 or 3.7.6)
 8. Virtualenv (20.0.17)
 
 ## 0. Install vagrant and VirtualBox:
 1. You need to go to the virtualbox [website](https://www.virtualbox.org/wiki/Downloads) and download the latest version for Windwos(I have it 6.1.30)<br>
 2. You need to go to the vagrant [website](https://www.vagrantup.com/downloads) and download the last version(for you processor 32/64-bit) for Windows(I have it 2.2.19 for 64-bit)

 ##  Install WSL2
   You must use WSL2. To install it, check the official documentation.<br>
   You need to turn on components WSL 10 using dism: <br>
   ```
   dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
   dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestar
   ```
   or using PowerShell: <br>
   ```
   Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
   Enable-WindowsOptionalFeature -Online -FeatureName VirtualMachinePlatform
   ```
   and then restart you PC;<br>
   Hardware virtualization support must be enabled in the BIOS / UEFI setup of the computer. <br>
   After these steps you need to update WSL for WSL 2 version. To do this, you need to go to the [website](https://docs.microsoft.com/ru-ru/windows/wsl/wsl2-kernel) and download    the wsl_update_x64.msi file, install it. <br>
 
   To make WSL2 the default architecture for new distributions, in PowerShell run the command:<br>
   ```
   wsl --set-default-version 2
   ```
   Then you need to install distribution (i have it Ubuntu 20.04).<br>
   And you can check the used WSL version in PowerShell using the command:<br>
   ```
   wsl -l -v
   ```
   if you don't see WSL version 2, you need using this comand:<br>
   ```
   wsl --set-version Ubuntu-20.04 2
   ```
   
## Install Vagrant inside WSL2
   Assuming you're using Ubuntu 20.04, run:
   ```
   # run inside WSL 2
   # check https://www.vagrantup.com/downloads for more info
   curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
   sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
   sudo apt-get update && sudo apt-get install vagrant
   ```
   Then, you must enable WSL 2 support. To do that, append two lines into the ~/.bashrc file: <br>
   ```
   # append those two lines into ~/.bashrc
   echo 'export VAGRANT_WSL_ENABLE_WINDOWS_ACCESS="1"' >> ~/.bashrc
   echo 'export PATH="$PATH:/mnt/c/Program Files/Oracle/VirtualBox"' >> ~/.bashrc

   # now reload the ~/.bashrc file
   source ~/.bashrc
   ```
   
##  Install virtualbox_WSL2 plugin
   If you have problem with Vagrant like this:
   ```
   Bringing machine 'default' up with 'virtualbox' provider...
==> default: Checking if box 'hashicorp/bionic64' version '1.0.282' is up to date...
==> default: Clearing any previously set forwarded ports...
==> default: Clearing any previously set network interfaces...
==> default: Preparing network interfaces based on configuration...
    default: Adapter 1: nat
==> default: Forwarding ports...
    default: 22 (guest) => 2222 (host) (adapter 1)
==> default: Booting VM...
==> default: Waiting for machine to boot. This may take a few minutes...
    default: SSH address: 127.0.0.1:2222
    default: SSH username: vagrant
    default: SSH auth method: private key
    default: Warning: Connection refused. Retrying...
    default: Warning: Connection refused. Retrying...
    default: Warning: Connection refused. Retrying...
    default: Warning: Connection refused. Retrying...
    default: Warning: Connection refused. Retrying...
    default: Warning: Connection refused. Retrying...
==> default: Waiting for cleanup before exiting...
   ```
   You need install plugin:
   ```
  $ vagrant plugin install virtualbox_WSL2
   ```
   
### Installation

Let's create a folder for our project.

```
  mkdir Task2
  cd Task2
```

After that, we will create a virtual environment.

```
  python3 -m venv Django_venv
```

Next, you need to install virtualenv.

```
  sudo apt-get install python3-venv -y
```

After that, you need to activate the virtual environment.
```
  source Django_venv/bin/activate
```

Useful link:
- [Website about Virtualenv]

After successfully isolating the project, you can create a Vagrantfile.

Vagrant is able to define and control multiple guest machines per Vagrantfile. 
Multiple machines are defined within the same project Vagrantfile using the config.vm.define method call. 
This configuration directive is a little funny, because it creates a Vagrant configuration within a configuration. 

My VagrantFile looks like this:
```
 Vagrant.configure("2") do |config|

  config.vm.define "postgre" do |config|            
    config.vm.hostname = "postgre"                 
    config.vm.box = "bento/ubuntu-20.04"                            
    config.vm.network "public_network", ip: "192.168.1.126", bridge: "Realtek 8821AE Wireless LAN 802.11ac PCI-E NIC"    of the machine
    config.vm.box_check_update = false                        
    config.vm.provider "virtualbox" do |vb|                   
            vb.cpus = 1                                              
            vb.gui = false                                          
            vb.memory = "2048"                                            
        
        end
  end

    config.vm.define "django" do |config|            
    config.vm.hostname = "django"                 
    config.vm.box = "bento/ubuntu-20.04"                          
    config.vm.network "public_network", ip: "192.168.1.184", bridge: "Realtek 8821AE Wireless LAN 802.11ac PCI-E NIC"     
    config.vm.box_check_update = false                          
    config.vm.provider "virtualbox" do |vb|                
            vb.cpus = 1                                           
            vb.gui = false                                          
            vb.memory = "2048"                                         
        
        end
  end
  
end

```

Useful links:
- [Multi-Machine]
- [Discover Vagrant Boxes]
- [Networking and Multimachine]

After that, we start both virtual machines. In my instructions, I will run them one by one.
We start the virtual machine on which Postgresql will be installed.
```
  vagrant up postgre
  vagrant ssh postgre;
```

### The main task

#### 1) Installing Postgresql
If you wish to install Postgresql, the process is very straightforward.
First, update your local package index with apt:
```
  sudo apt-get update
```
> PostgreSQL is available in all Ubuntu versions by default. 
> However, Ubuntu "snapshots" a specific version of PostgreSQL that is then supported throughout the lifetime of that Ubuntu version. 
> Other versions of PostgreSQL are available through the PostgreSQL apt repository.
To use the apt repository, follow these steps:

Create the file repository configuration:
```
  sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
```
Import the repository signing key:
```
  wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
```
Update the package lists:
```
  sudo apt-get update
```
Install the latest version of PostgreSQL.
```
  sudo apt-get -y install postgresql
```
Now postgresql is installed on the virtual machine.
Run the command that will show the status of the PostgreSQL service.
```
  systemctl status postgresql
```
Now you need to creating ssl certificates.

To create a simple self-signed certificate for the server, valid for 365 days, use the following OpenSSL command, replacing 'dbhost.yourdomain.com' with the server's host name:
```
  openssl req -new -x509 -days 365 -nodes -text -out server.crt \
  -keyout server.key -subj "/CN=dbhost.yourdomain.com"
```
Then do:
```
  chmod og-rwx server.key
```
because the server will reject the file if its permissions are more liberal than this. For more details on how to create your server private key and certificate.
While a self-signed certificate can be used for testing, a certificate signed by a certificate authority (CA) (usually an enterprise-wide root CA) should be used in production.
To create a server certificate whose identity can be validated by clients, first create a certificate signing request (CSR) and a public/private key file:
```
  openssl req -new -nodes -text -out root.csr \
    -keyout root.key -subj "/CN=root.yourdomain.com"
  chmod og-rwx root.key
  ```
  Then, sign the request with the key to create a root certificate authority (using the default OpenSSL configuration file location on Linux):
  ```
    openssl x509 -req -in root.csr -text -days 3650 \
  -extfile /etc/ssl/openssl.cnf -extensions v3_ca \
  -signkey root.key -out root.crt
  ```
  Finally, create a server certificate signed by the new root certificate authority:
```
  openssl req -new -nodes -text -out server.csr \
    -keyout server.key -subj "/CN=dbhost.yourdomain.com"
  chmod og-rwx server.key

  openssl x509 -req -in server.csr -text -days 365 \
    -CA root.crt -CAkey root.key -CAcreateserial \
    -out server.crt
```
server.crt and server.key should be stored on the server, and root.crt should be stored on the client so the client can verify that the server's leaf certificate was signed by its trusted root certificate. root.key should be stored offline for use in creating future certificates.

For sending root.crt on server we will use scp technology (copying files via ssh).
You can check the ssh connection:
```
  ssh vagrant@192.168.1.184
  exit
```
> vagrant@192.168.1.184's password:
>
> password - vagrant

Copy a local file to the server:
```
  scp root.crt vagrant@192.168.1.184:/home/vagrant/example
```

Next, you need to configure postgresql.
We allow connection to PostgreSQL over the network.
Open the postgresql.conf file
```
  nano /etc/postgresql/14/main/postgresql.conf
```
We find the following line:
```
  #listen_addresses = 'localhost'
  
  ssl = on
  #ssl_ca_file = ''
  ssl_cert_file = ''
  #ssl_crl_file = ''
  #ssl_crl_dir = ''
  ssl_key_file = '' 
```
We make the following changes:
```
  listen_addresses = '*'
  
  ssl = on
  #ssl_ca_file = ''
  ssl_cert_file = '/etc/postgresql/14/main/server.crt'
  #ssl_crl_file = ''
  #ssl_crl_dir = ''
  ssl_key_file = '/etc/postgresql/14/main/server.key'
```
> Instead of '*', you can specify the IP address of the required interface

Now let's allow the connection from the network.
Open the pg_hba.conf file.
```
  nano /etc/postgresql/14/main/pg_hba.conf
```

Restarting PostgreSQL for the changes to take effect.
```
  systemctl restart postgresql
```
Creating a user and database in PostgreSQL.
Switch to the postgres user.
```
  su - postgres
```
Run the psql utility – this is a console for PostgreSQL.
```
  psql
```
First of all, we need to set a password for the postgres user.
```
  \password postgres
```
Creating a new user on the PostgreSQL server.
```
  create user vlad with password '1111Aa';
```
> taras is the username, ‘120360A’ is his password.
Let's create a database.
```
  create database mydb;
```
> django_db_taras is the name of the new database.
We will transfer the database management rights to our new user.
```
  grant all privileges on database mydb to vlad;
  \q
```
To check, let's connect to PostgreSQL on behalf of a new user.
```
  psql -h localhost mydb vlad
```
Everything works.
```
  \q
  exit
```
Postgresql is installed and configured. 



Useful links:
- [PostgreSQL Apt]
- [Installing and configuring PostgreSQL]
- [scp]

#### 2) Installing django
Then we can proceed with the installation of Django.
We are launching a virtual machine on which Django will be installed.
```
  vagrant up django
  vagrant ssh django
```
If you wish to install Django using the Ubuntu repositories, the process is very straightforward.
First, update your local package index with apt:
```
  sudo apt update
```

Install Python and PIP3
```
  sudo apt install python3 python3-pip
```
You can check the installed version of PIP with the following command:
```
  pip3 -V
```
Install Django
Once PIP is installed, you can start installing Django.
```
  sudo pip3 install Django
```
Then, check the installed version.
```
  django-admin --version
```
Create a new Django project
I will create a new project called example and install psycopg2.
 
```
  sudo django-admin startproject example
  sudo apt install python3-psycopg2
```
Next, navigate into the example folder and migrate to set the initial configuration.
```
  cd example 
  sudo python3 manage.py migrate
```
Also, you have to create the superuser to manage the project.
```
  sudo python3 manage.py createsuperuser
```
The next step is to edit the project configuration file to make it accessible from any computer. 
So, open the file settings.py.
```
  sudo nano example/settings.py
```
And edit the following entry:
```
  ALLOWED_HOSTS = ['COMPUTER_IP_WHICH_IS_RUNNING_DJANGO']
```
And also configure the DATABASES section of the project configuration file settings.py:
```
  DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql_psycopg2',
        'NAME': 'mydb',
        'USER' : 'vlad',
        'PASSWORD' : '1111Aa',
        'HOST' : '192.168.1.126',
        'PORT' : '5432',
        'OPTIONS': {
        'sslrootcert': '/home/vagrant/example/root.crt',
        },
    }
}
```
Navigate into the example folder and migrate to set the initial configuration.
```
  cd example 
  sudo python3 manage.py migrate
```
Next, serve the project:
```
  sudo python3 manage.py runserver 0.0.0.0:8000
```
> Finally, access to your project using the web browser using the IP address of the computer which is running Django. 
> Remember, Django uses the 8000 port, so you have to put it on the URL.
> Also, you can log in to access the admin panel. http://your-server:8000/admin

Useful links:
- [Install Django]
- [How To Install the Django]
- [Django+PostgreSQL]


For verification, you can create a new user on the website and then log in to the postgresql database:
```
  psql postgresql://vlad@localhost:5432/mydb
```
Make the following sql query
```
  select * from auth_user;
```
You should see the credentials you saw on the website.




[Website about Virtualenv]: https://blog.sedicomm.com/2021/06/29/chto-takoe-virtualenv-v-python-i-kak-ego-ispolzovat/
[Multi-Machine]: https://www.vagrantup.com/docs/multi-machine
[Discover Vagrant Boxes]: https://app.vagrantup.com/boxes/search?utf8=%E2%9C%93&sort=downloads&provider=&q=bento+ubuntu+20.04
[Networking and Multimachine]: https://nextheader.net/2018/04/24/vagrant-part-iii-networking-and-multimachine/
[Install Django]: https://www.osradar.com/install-django-on-debian-10/
[How To Install the Django]: https://www.digitalocean.com/community/tutorials/how-to-install-the-django-web-framework-on-ubuntu-20-04
[PostgreSQL Apt]: https://www.postgresql.org/download/linux/ubuntu/
[Installing and configuring PostgreSQL]: https://info-comp.ru/install-postgresql-12-on-debian
[Django+PostgreSQL]: https://www.djbook.ru/examples/77/
[scp]: https://wiki.enchtex.info/tools/console/scp
