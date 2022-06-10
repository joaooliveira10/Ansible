# Ansible

Links
  Ansible: https://docs.ansible.com/ansible/latest/index.html
  Chocolatey: https://chocolatey.org/install
  Pip: https://packaging.python.org/en/latest/guides/installing-using-linux-tools/

1.Installation Ubuntu Virtual Machine
  1.1. Windows native virtualization option
    Open PowerShell
      wsl -l -v                      #Check if you have wsl and what version
      wsl --install -d ubuntu        #Ubuntu Installation
 
 2.1. Virtualization option by VirtualBox
  Install VirtualBox
  Install Ubuntu image

2.Windowns preparation
 1.1. Open PowerShell
	Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))

3.Ubuntu [Master]
  1.1.Open Terminal
  
  2.1.Install Ansible
    sudo apt update
    sudo apt install software-properties-common
    sudo add-apt-repository --yes --update ppa:ansible/ansible
    sudo apt install ansible
    
  2.2.Install Ansible "not a better option"
    yum install ansible -y
    
  3.1.pip
    sudo apt update
    sudo apt install python3-venv python3-pip
    
  4.1 lsb_release -a

  5.1 apt list -a ansible

  6.1 apt list -a python3

  7.1 pip list | grep pywinrm

  8.1 ansible-galaxy collection install chocolatey.chocolatey


5.Windows [Client]
  1.1.Check the ip of the new machine
Windows Option = ipconfig / Linux Optino = ip addr show 
"10.99.103.163"

5.Hosts Editor [Master]
  1.1.Open Text Editor(Use vi, nano or gedit...)
    vi /etc/ansible/hosts
  2.1.Edit host

[windows]
10.99.103.163

[windows:vars]
ansible_user="Login123"
ansible_password="Password123"
ansible_port=5986
ansible_connection=winrm
ansible_winrm_server_cert_validation= ignore


6.Edit install.yml [Master]

  1.1. Terminal(Use vi, nano or gedit...)
    vi install.yml
    
  2.2.Edit install.yml
- hosts: windows
  gather_facts: true
  tasks:
  - name: Install firefox
    win_chocolatey:
      name: firefox
      state: present

7.Check the syntax of the install.yml ansible playbook
  1.1 ansible-playbook install.yml --syntax-check

8.Run the install.yml playbook
  1.1 ansible-playbook install.yml

Extra

E.1.Create playbook for uninstall application
  1.1 vi uninstall.yml
- hosts: windows
  gather_facts: true
  tasks:
  - name: Install firefox
    win_chocolatey:
      name: firefox
      state: absent

E.2.Check the syntax of the uninstall.yml ansible playbook
  1.1 ansible-playbook uninstall.yml --syntax-check

E.3.Run the uninstall.yml playbook
  1.1 ansible-playbook uninstall.yml
  
  
  
