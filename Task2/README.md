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
 9. 
 
 ## 0. Install vagrant and VirtualBox:
 1. You need to go to the virtualbox [website](https://www.virtualbox.org/wiki/Downloads) and download the latest version for Windwos(I have it 6.1.30)<br>
 2. You need to go to the vagrant [website](https://www.vagrantup.com/downloads) and download the last version(for you processor 32/64-bit) for Windows(I have it 2.2.19 for 64-bit)

 ## 1. Install WSL2
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
   
## 2. Install Vagrant inside WSL2
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
   
## 3. Install virtualbox_WSL2 plugin
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
## 4. Create project folder:
   Go to the required directive and create project folder:
   ```
   $ mkdir VagrantVM
   $ cd ./Task2/
   ```
## 5. Create your virtualenv:
   First, verify the installed Python version:
   ```
   # check Python version
   $ python3 -V
   Python 3.8.10
   $ python3 -m venv virtualenv
   $ ls
   virtualenv   
   ```
## 6. Connect to postgres machine via ssh:
 
   ```
  
   ```
## 7. Connect to VM:
   ```
   $ vagrant ssh ntp.edu.tentixo.com
   ```
## 8. Checking the work of ntp servers:
   And run these commands in rhel8:
   ```
    $ chronyc sources
    $ chronyc tracking
   ```
# About files:
## 1. Vagrant files:
   ```
#This line is responsible for the name of the configuration and version vagrant.
Vagrant.configure("2") do |config|

  config.vm.define "ntp.edu.tentixo.com" do |config| 			
    config.vm.hostname = "ntp.edu.tentixo.com"				
    config.vm.box = "generic/rhel8"					
    config.vm.network "private_network", ip: "192.168.56.2"		
    config.vm.box_check_update = false					
    
  end
  
    config.vm.provider "virtualbox" do |vb|				 
      vb.cpus = 1							 
      vb.gui = false							 
      vb.memory = "1024"						
      
    end
    
    config.ssh.insert_key = false					
    config.vm.provision :ansible do |ansible|				
    	ansible.playbook = "playbook.yml"					
    	ansible.inventory_path = "inventory"				
    	ansible.raw_arguments = ["-vvv", "--flush-cache"
	]								 
	
    end
    
end
   ```
## 2. Playbook.yml file:
   ```
   ---
- hosts: ntp										
  become: true										
  tasks: 

    - name: Update Operation system							
      package:
        name: '*'
        state: latest

    - name: Copy line the chrony configuration
      lineinfile:									
        path: /etc/chrony.conf
        regexp: '{{item.regexp}}'							
        line: '{{item.line}}'								
        backrefs: yes									
      with_items:
        - {regexp: '^pool(.*)', line: 'server sth1.ntp.se \n server sth4.ntp.se' }	

    - name: Restart Chrony service							
      systemd:
        name: chronyd
        state: restarted
   ```
## 3. About inventory file:
   ```
    You can read more information about inventory file here:
    https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html
   ```
## 4. About requirements.txt file:
   ```
   Good article about requirements.txt:
   https://blog.sedicomm.com/2021/06/29/chto-takoe-virtualenv-v-python-i-kak-ego-ispolzovat/
   ```
# Useful links
  I faced some problems or just new information, these sites helped me to solve them:<br>
  1.https://docs.ansible.com/ansible/2.9/modules/systemd_module.html<br>
  2.https://docs.ansible.com/ansible/2.8/user_guide/playbooks_best_practices.html<br>
  3.https://github.com/Karandash8/virtualbox_WSL2<br>
  4.https://stackoverflow.com/questions/40535667/ansible-failed-to-connect-to-the-host-via-ssh<br>
  5.https://www.schakko.de/2020/01/10/fixing-unprotected-key-file-when-using-ssh-or-ansible-inside-wsl/<br>
  6.https://stackoverflow.com/questions/41377375/failed-to-connect-to-host-via-ssh-on-vagrant-with-ansible-playbook<br>
  7.https://runebook.dev/ru/docs/ansible/collections/ansible/builtin/lineinfile_module<br>
