# LABORATORY-ANSIBLE

#### Container

Keep a docker running

```bash
ENTRYPOINT ["tail", "-f", "/dev/null"]
```

Connect to controller

```bash
$ sudo docker exec -it labo-controller /bin/bash
```

### AWS EC2

![./documentation/1.png](./documentation/1.png)

I have created a inventory with two nodes representing two different servers EC2.

```
[nodes]
ec2-18-237-32-197.us-west-2.compute.amazonaws.com
ec2-54-214-124-124.us-west-2.compute.amazonaws.com
[nodes:vars]
ansible_user=ec2-user
ansible_ssh_private_key_file=/home/ansible/aws.pem
```

- Run a command and show the result in output

```yml
--- # Do things on nodes
- hosts: nodes
  become: yes
  tasks:
    - name: install git
      yum:
        name: git
        state: latest
    - name: run script
      command: sh ansible.sh
      register: command_output
      args:
        chdir: scripts/
    - debug:
        var: command_output.stdout_lines
```

- Copy files from controller to nodes

```yml
--- # Copy file from master to remote
- hosts: nodes
  become: yes
  tasks:
    - name: copy file
      copy: src=/home/ansible/testcopy
        dest=/home/ansible/trol
```
