# :desktop_computer: ansible-comunicacao-dados
  Projeto da disciplina de Comunicação de Dados para instalação de softwares em múltiplos computadores através de um servidor utilizando o Ansible.

## Conteúdo

1. [Introdução](https://github.com/joaooliveira10/Ansible/blob/main/README.md#introdu%C3%A7%C3%A3o)
2. [O que é o Ansible?](https://github.com/joaooliveira10/Ansible/blob/main/README.md#o-que-%C3%A9-o-ansible)
3. [Ferramentas utilizadas](https://github.com/joaooliveira10/Ansible/blob/main/README.md#ferramentas-utilizadas)
5. [Passo a passo da configuração](https://github.com/joaooliveira10/Ansible/blob/main/README.md#passo-a-passo-da-configura%C3%A7%C3%A3o)
7. [Referências](https://github.com/joaooliveira10/Ansible/blob/main/README.md#refer%C3%AAncias)

## Introdução

  A ideia para este projeto foi delineada visando a otimização da configuração das máquinas dos laboratórios de informática do IFMT - Campus Octayde para o início das aulas das turmas anuais e semestrais. Para tanto, o objetivo é criar um servidor com uma máquina central e realizar a instalação de múltiplos programas simultâneamente em todos os computadores dos laboratórios.

## O que é o Ansible?

  O Ansible é uma ferramenta open source para automação de processos manuais de TI como instalação de softwares, provisionamento de infraestrutura, compartilhamento de automação e configurações através da mesma rede, etc.
  
## Ferramentas utilizadas

  - Para o servidor Linux:
    - Ansible;
    - Python3-pip;
    - WinRM

  - Para as máquinas Windows:
    - PowerShell;
    - Chocolatey;

## Passo a passo da configuração

  A primeira etapa a ser realizada é a instalação e configuração de uma máquina, física ou virtual, com o Sistema Operacional Linux para a função de servidor de rede do projeto.
  
  Caso seja utilizada uma máquina virtual, faça o download e instalação do [Virtual Box](https://www.virtualbox.org/wiki/Downloads) e siga as instruções deste [link](https://canaltech.com.br/software/como-criar-uma-maquina-virtual-com-o-virtualbox/), ou se preferir, há também a opção de virtualização nativa para o Windows. Para isso, abra o PowerShell como administrador e insira os seguintes comandos:
  
  ```
  wsl -l -v
  wsl --install -d ubuntu	
  ```

  O próximo passo é preparar o Windows da máquina. Para tanto, execute no PowerShell:
  
  ```
  Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
  ```
  
  Com o Linux instalado, abra o terminal e inicie a instalação do Ansible e do gerenciador de pacotes pip:
 
 ```
 sudo apt update
 sudo apt install software-properties-common
 sudo add-apt-repository --yes --update ppa:ansible/ansible
 sudo apt install ansible
 ```
 
 ```
 yum install ansible -y
 ```
 
 ```
 sudo apt update
 sudo apt install python3-venv python3-pip
 ```
 
  Em seguida, abra o editor de texto para editar o arquivo de host:

```
 vi /etc/ansible/hosts
```

```
 hosts: windows
  gather_facts: true
  tasks:
   name: Install firefox
    win_chocolatey:
     name: firefox
     state: present
```

Agora, em um outro computador, verifique o seu ip no terminal:

```
ip addr show / ipconfig
```

Por fim, volte para a máquina de servidor e abra o arquivo install.yml com o editor de texto:

```
vi install.yml
```

```
[windows]
10.99.103.163

[windows:vars]
ansible_user="Conta123"
ansible_password="senha123"
ansible_port=5986
ansible_connection=winrm
ansible_winrm_server_cert_validation= ignore
```

## Referências

RED HAT. ***O que é o Ansible?*** Disponível em: <<https://www.redhat.com/pt-br/technologies/management/ansible/what-is-ansible>>

LINUX HELP. ***How to install an application on Windows by using Ansible playbook.*** Disponível em: <<https://www.linuxhelp.com/how-to-install-an-application-on-windows-by-using-ansible-playbook>>

ANSIBLE DOCUMENTATION. ***Setting up a Windows Host.*** Disponível em: <<https://docs.ansible.com/ansible/latest/user_guide/windows_setup.html>>

