"# SofTteco_DevSecOps" 
<br>
## Task: <br>
 Change ntp to ntp.se with Ansible in an RHEL 8 virtual server named ntp.edu.tentixo.com via VagrantBox on VirtualBox - connection should be done with computer name, not IP.<br>
 
 [-Info about RHEL8](https://www.linuxadictos.com/ru/rhel8.html) <br>
 [-Info about VirtualBox](https://www.virtualbox.org/)<br>
 [-Info about VagrantBox](https://www.vagrantup.com/)<br>
 [-Info about Ansible](https://www.thomaspreischl.de/ansible-wsl-windows/)<br>
 [-Info about WSL2](https://winitpro.ru/index.php/2020/07/13/zapusk-linux-v-windows-wsl-2/)<br>
 
 ## Objective of the project: <br>
 The purpose of the work is to acquire practical skills in working with Ansible, VagrantBox, VirtualBox, RHEL8, WSL2 and automatic configuration of ntp servers on ntp.se  <br>
 
 ## Requirements:
 1. Windows 10 - version +19042.928
 2. VirtualBox - version +6.1.22 (Windows version)
 3. WSL 2
 4. Vagrant +2.2.18 (Linux version)
 5. Vagrant plugin: virtualbox_WSL2
 6. (maybe) PowerShell Preview
 
 ## 0. Install vagrant and VirtualBox:
 1. You need to go to the virtualbox [website](https://www.virtualbox.org/wiki/Downloads) and download the latest version for Windwos(I have it 6.1.30)<br>
 2. You need to go to the vagrant [website](https://www.vagrantup.com/downloads) and download the last version(for you processor 32/64-bit) for Windows(I have it 2.2.19 for 64-bit)

 ## 1. Install WSL2
   You must use WSL2. To install it, check the official documentation.<br>
   1. You need to turn on components WSL 10 using dism: <br>
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
   
## 2. Install PowerShell Preview
   Depending on your Windows version, you may need to install the PowerShell Preview version. If that's the case, go to your current PowerShell version and run the following     command:<br>
   ```
   Invoke-Expression "& { $(Invoke-Restmethod https://aka.ms/install-powershell.ps1) } -UseMSI -Preview"
   ```
   Go through all the steps and finish the installation process. <br>
   
## 3. Install Vagrant inside WSL2
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
   
## 4. Install virtualbox_WSL2 plugin
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
## 5. Create project folder:
   ```
   $ mkdir VagrantVM
   $ cd ./VagrantVM/
   ```
### 6. Create your virtualenv:
   ```
   # Clone Git repository with my project
        $ git clone https://gitlab.com/Dmitry.Plikus/devsecops.git 
        $ cd ./devsecops/
        $ python3 -m venv devsecops
        $ source devsecops/bin/activate
        # Install packages
        $ pip install -r requirements.txt
   ```
### 7. Install ansible and Vagrant build:
   ```
   $ apt install ansible
   $ vagrant up
   ```
### 8. Connect to VM:
   ```
   $ vagrant ssh ntp.edu.tentixo.com
   ```
### 9. Checking the work of ntp servers:
   ```
    $ chronyc sources
    $ chronyc tracking
   ```

   
 
 

