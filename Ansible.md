## Ansible Documentation

 

## Switch to root user and update system
```
sudo su -
yum update -y

```
## Install Ansible
```
amazon-linux-extras install ansible2 -y

```
or 
```
yum install ansible -y

```
## To check Ansible version
```
ansible --version

```
## Create Ansible Playbook
```
vi local_playbook.yml

```

 

## PLAYBOOK
```
---
- name: Install Maven, Git, clone the code, and build
  hosts: localhost  # Change 'web1' to 'localhost'
  connection: local  # Use a local connection

 

  tasks:
    - name: Install Git
      package:
        name: git
        state: present

 

    - name: Install Java Development Kit (JDK) 11
      become: yes
      ansible.builtin.yum:
        name: java-11-amazon-corretto
        state: present

 

    - name: Install Maven
      ansible.builtin.yum:
        name: maven
        state: present

 

    - name: Clone the Git repository to 'ecomapp' directory
      ansible.builtin.git:
        repo: "https://github.com/Likhi-G1/ecomapp.git"
        dest: "{{ ansible_env.HOME }}/ecomapp"
        accept_hostkey: yes

 

    - name: Build the application with Maven
      ansible.builtin.command: "mvn clean install"
      args:
        chdir: "{{ ansible_env.HOME }}/ecomapp"

 

    - name: Install Docker
      ansible.builtin.yum:
        name: docker
        state: present

 

    - name: Start Docker service
      ansible.builtin.service:
        name: docker
        state: started
        enabled: yes

 

    - name: Build Docker image
      ansible.builtin.command: "docker build -t ecomapp:latest ."
      args:
        chdir: "{{ ansible_env.HOME }}/ecomapp"  # Change this to the appropriate path

 

    - name: Download Docker Compose binary
      ansible.builtin.get_url:
        url: "https://github.com/docker/compose/releases/download/v2.5.0/docker-compose-linux-x86_64"
        dest: /usr/local/bin/docker-compose
        mode: '0755'

 

    - name: Create symlink for Docker Compose
      ansible.builtin.file:
        src: /usr/local/bin/docker-compose
        dest: /usr/bin/docker-compose
        state: link

 

    - name: Update YUM packages
      ansible.builtin.command: "sudo yum update -y"

 

    - name: Start Docker Compose
      ansible.builtin.command: "docker-compose up -d"
      args:
        chdir: "{{ ansible_env.HOME }}/ecomapp"  # Change this to the appropriate path

 

```

 

## To run playbook on local host
```
ansible-playbook -i localhost, -c local local_playbook.yml

```

 

# Configuring Remote Host:

## To create Ansible Inventory file
```
vi inventory.ini

```
## INVENTORY FILE
```
[web_servers]
web1 ansible_host=34.221.176.104 ansible_user=ec2-user ansible_ssh_private_key_file=keys/ansible.pem

```
## clone keypair from github
```
git clone https://github.com/Likhi-G1/keys.git

```
## Change permissions for SSH authentication 
```
chmod 600 keys/ansible.pem

```
## Create Ansible Playbook
```
vi remote_playbook.yml

```

## PLAYBOOK
```
---
- name: Install Maven, Git, clone the code, and build
  hosts: web1
  become: yes

  tasks:
    - name: Install Git
      package:
        name: git
        state: present

    - name: Install Java Development Kit (JDK) 11
      ansible.builtin.yum:
        name: java-11-amazon-corretto
        state: present

    - name: Install Maven
      ansible.builtin.yum:
        name: maven
        state: present

    - name: Clone the Git repository to 'ecomapp' directory
      ansible.builtin.git:
        repo: "https://github.com/Likhi-G1/ecomapp.git"
        dest: "{{ ansible_env.HOME }}/ecomapp"
        accept_hostkey: yes

    - name: Build the application with Maven
      ansible.builtin.command: "mvn clean install"
      args:
        chdir: "{{ ansible_env.HOME }}/ecomapp"

    - name: Install Docker
      ansible.builtin.yum:
        name: docker
        state: present

    - name: Start Docker service
      ansible.builtin.service:
        name: docker
        state: started
        enabled: yes

    - name: Build Docker image
      ansible.builtin.command: "docker build -t ecomapp:latest ."
      args:
        chdir: "{{ ansible_env.HOME }}/ecomapp"  # Change this to the appropriate path

    - name: Download Docker Compose binary
      ansible.builtin.get_url:
        url: "https://github.com/docker/compose/releases/download/v2.5.0/docker-compose-linux-x86_64"
        dest: /usr/local/bin/docker-compose
        mode: '0755'

    - name: Create symlink for Docker Compose
      ansible.builtin.file:
        src: /usr/local/bin/docker-compose
        dest: /usr/bin/docker-compose
        state: link

    - name: Update YUM packages
      ansible.builtin.command: "sudo yum update -y"

    - name: Start Docker Compose
      ansible.builtin.command: "docker-compose up -d"
      args:
        chdir: "{{ ansible_env.HOME }}/ecomapp"  # Change this to the appropriate path

```

 

## To run playbook on remote host
```
ansible-playbook -i inventory.ini -l web1 remote_playbook.yml

```

## To install ansible on windows
```
pip install ansible

```
