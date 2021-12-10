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
 
 ## Installation and use processes:
 This project work on Windows 10 OS using WSL2(Only WSL2!!!) and in WSL2 using Ubuntu 20.04 <br>
 
 ## 0. Install vagrant and VirtualBox:
 1. You need to go to the virtualbox [site](https://www.virtualbox.org/wiki/Downloads) and download the latest version for Windwos(I have it 6.1.30)<br>
 2. You need to go to the vagrant [site](https://www.vagrantup.com/downloads) and download the last version(for you processor 32/64-bit) for Windows(I have it 2.2.19 for 64-bit)

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
   
 
 

