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

[databases]
3.84.24.25
54.89.141.43
```

# Testing
- Try to ping all servers using the in-built `all` group
- use `--key-file` or `--private-key=/home/to/key.pem` to connect for now
```bash
$ ansible -i inventory.ini --key-file /home/to/private_key.pem/ -u ubuntu -m ping all
```
- I'm using `ubuntu` as `user -u` because it's an ubuntu server. Different distros have their unique user IDs