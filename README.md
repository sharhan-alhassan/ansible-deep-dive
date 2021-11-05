# ansible-deep-dive
A deep-dive into the inner workings of ansible-playbooks with real-life application scenario

# Playbook modules:
- Playbooks uses
```bash
1. "package" module -- eg; for updates
2. "service" module -- eg; Nginx
3. "user" module -- add/remove Linux users
```

# Inventory
- contains IPs or hostnames
- dynamic scripts talk with APIs, eg; An inventory that talk with all your EC2 Instances.

# Inventory setup
```bash
[webservers]
3.84.141.162	
3.84.145.218	

[dbservers]
3.84.24.25
54.89.141.43
```

# Testing
- Try to ping all servers using the in-built `all` group module
- use `--key-file` or `--private-key=/home/to/key.pem` to connect for now
```bash
$ ansible -i inventory.ini --key-file /home/to/private_key.pem/ -u ubuntu -m ping all
```
- I'm using `ubuntu` as `user -u` because it's an ubuntu server. Different distros have their unique user IDs

# Testing group of servers
```bash
$ ansible -i inventory.ini --key-file /home/to/private_key.pem/ -u ubuntu -m ping webservers
```

# ad-hoc commands
- A command you type to get something done quick which you don't want to save for later
- Let's use the `user module` (which manages system users) ad-hoc command to add and remove a user in all the servers
- use `-b` on the command line to elevate privileges to `root`
```bash
$ ansible -i inventory.ini --private-key=/home/to/key.pem/ -u ubuntu -m user -a "name=sam  state=present" all
``` 

# Using package to make installations
- Let's install `nginx` on the webservers by using `name=nginx` and `state=present`
- Elevate privileges to root with `-b`. It's basically running: `sudo apt-get install nginx`
- To uninstall the nginx service, set `state=absent`

# Playbooks
- They're ansible configuration, deployment and orchestration language
- Let's configure `Webservers` and `Database servers`

- `Webservers`
```yml
- name: Configure webservers
  hosts: webservers
  become: yes
  tasks:
    - name: Create non-root user
        user:
          name: sam
          state: present
    - name: Install nginx
        package:
          name: nginx
          state: present
```
- `Databse servers`
```yml
- name: Configure database servers
  hosts: dbservers
  become: yes
  tasks:
    - name: Install MySQL Server
      package:
        name: mysql-server
        state: present 
```
## Playbooks explanation: Webservers
- We have two playbooks named `Configure webservers` and `Configure database servers`
- First playbook, Webservers targets the `webservers` hosts group and assume sudo user/root user with `become=yes`.
- It performs two tasks. 
- First task uses the `user module` to create a superuser `sam`
- Second task uses the `package module` to install ningx

## Database servers
- Second playbook, Database servers targets the `dbservers` hosts group and assume sudo user/root user with `become=yes`.
- It performs one tasks. 
- The task uses the `package module` to install `mysql-server`

# How to Run a Playbook
- `ansible-playbook <playbook-name> -i <inventory-name> -u <user-name>`
```bash
$ ansible-playbook main.yml -i inventory -u ubuntu
```