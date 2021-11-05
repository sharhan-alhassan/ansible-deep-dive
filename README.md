
# Key values in ansible
- Taking a variable as a key-value pair
```yml
- name: variables...
  hosts: webservers
  gather_facts: no
  vars:
    hello: world
    ournumberlist: [1,2,3,4]
    ourstringlist:
      - "a"
      - "b"
      - "c"
  tasks:
    - debug:
        msg: "Hello, {{ hell }}"
```

- Taking variables from `mapping`
```yml
- name: variables...
  hosts: webservers
  gather_facts: no
  vars:
    hello: world
    ournumberlist: [1,2,3,4]
    ourstringlist:
      - "a"
      - "b"
      - "c"
    ourmap:
      hi: sharhan
      goodbye:
        
  tasks:
    - debug:
        msg: "Hello, {{ ourmap.hi }}"   # will print: Hello, sharhan

```