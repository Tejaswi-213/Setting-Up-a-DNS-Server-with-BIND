# Setting-Up-a-DNS-Server-with-BIND

Setup and Configuration:
Install Ansible and SSH:
sudo apt update
sudo apt install ansible openssh-client openssh-server

Authenticate with the Server:
ssh-keygen -t rsa
ssh-copy-id user@dns_server

Define Ansible Playbooks:
Create a Playbook for VM Setup:
- name: Set up DNS server VM
  hosts: localhost
  tasks:
    - name: Create virtual machine
      azure_rm_virtualmachine:
        resource_group: myResourceGroup
        name: myDNSVM
        vm_size: Standard_B1s
        admin_username: azureuser
        admin_password: Password1234!
        image:
          offer: UbuntuServer
          publisher: Canonical
          sku: 18.04-LTS
          version: latest

Create a Playbook for BIND Installation and Configuration:
- name: Configure DNS server with BIND
  hosts: myDNSVM
  become: yes
  tasks:
    - name: Install BIND
      apt:
        name: bind9
        state: present

    - name: Configure BIND
      copy:
        src: /path/to/named.conf.options
        dest: /etc/bind/named.conf.options

    - name: Start BIND
      service:
        name: bind9
        state: started
        enabled: yes

Run the Playbooks:
Execute the playbooks to set up the infrastructure and deploy the DNS server:
ansible-playbook setup_vm.yml
ansible-playbook configure_dns_server.yml
