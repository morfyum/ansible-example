### 1. Install python3 tools for Ansible because Ubuntu ansible version is so old.

```
sudo apt install python3 python3-simplejson python3-apt python3-pip python3-virtualenv
```

### 2. Createa python pip virtaulenv for ansible

```
python3 -m virtualenv ansible		# Create a virtualenv if one does not already exist
source ansible/bin/activate		# Activate the virtual environment
pip3 install ansible			# pip (python) or pip3 (python3) install
```
```deactivate 				# Exit from source ansible/bin/activate```

### 3. Ansible need sudo privilege!
Option 1: NOPASSWD for sudo
Option 2: Create a new user with sudo, and give NOPASSWD for this user only! (Better option)
`sudo cat /etc/sudoers`

`sudo visudo`
Example for Option 1:
```
%sudo   ALL=(ALL:ALL) ALL
and look like this:
%sudo  ALL=(ALL:ALL) NOPASSWD: ALL
```
Example for Option 2:
Create a new user `ansibleuser` and add this line to `/etc/sudoers`
`%ansibleuser ALL=(ALL:ALL) NOPASSWD: ALL`

### 4. Recommended 1 ssh-key per environment!
```ssh-keygen -t rsa -b 4096```
```ssh-copy-id -i ~/.ssh/mykey user@host```

```
# Create inventory.yml
all:
  children:
    servers:
	  hosts:
	    ansible-server:
		  ansible_host: 10.1.1.233
		  ansible_port: 22
		  ansible_user: mars
```

### Create Roles


### Create playbook.yml
```
- name ping server
  hosts: servers		# is a "servers" line in inventory.yml?
  tasks:
    - name: ping server
	  ping:
```

### Role files
  LINK
    - https://docs.ansible.com/ansible/latest/user_guide/playbooks_reuse_roles.html


### Run playbook on inventory servers 
```
ansible-playbook -i inventory.yml playbook.yml
```

### MULTILINE ROW REPLACE
  LINK:
    - https://www.middlewareinventory.com/blog/ansible-lineinfile-multiple-lines-replace-multiple-lines/
```
---
  - name: Examples of lineinfile
    hosts: web
    tasks:
      - name: "Ansible Lineinfile Multiple Lines"
        become: yes
        become_user: root
        lineinfile:
          path: /etc/httpd/conf/httpd.conf
          # Line to Search/Match against
          regexp: '{{item.From}}'
          # Line to Replace with
          line: '{{item.To}}'
          state: present  
          # To validate the Changes Before Committing/Saving
          validate: "httpd -t -f %s"
        with_items:
         - { From: 'ServerName www.middlewareinventory.com:80', To: 'ServerName www.devopsjunction.com:80'}
         - { From: 'ServerAdmin aksarav@middlewareinventory.com', To: 'ServerAdmin aksarav@devopsjunction.com'}
         - { From: '^LogLevel .*', To: 'LogLevel debug'}
         - { From: '^DocumentRoot .*', To: 'DocumentRoot "/var/www/devopsjunction"'}
```
