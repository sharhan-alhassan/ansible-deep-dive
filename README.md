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