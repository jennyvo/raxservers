# rax-servers
create, resize, delete servers

1-Spin up a vagrant box
````
$ vagrant up
$ vagrant ssh
````
2-create ansible_hosts
````
$ echo "127.0.0.1" > ~/ansible_hosts
````
3-update .raxpub ins src folder with user credential 

4-install some missing package if need
````
$ sudo apt-get install python-pip python-dev build-essential
$ sudo pip install --upgrade pip
```` 
5-run install ansible_requirements.txt from src folder
````
$ sudo pip install -r ansible_requirements.txt
````
6-check ansible version
````
$ ansible --version
```` 
7-setup authorization key
````
$ ssh-keygen -t rsa
$ cp ~/.ssh/id_rsa.pub ~/.ssh/authorized_keys (make sure you don't overwrite the existing keys in authorized_keys)
````
8-test
````
$ ansible all -m ping
  127.0.0.1 | success >> {
    "changed": false, 
    "ping": "pong"
  }
````
9-modify inventory/testgroup1/master/group_vars/all file to change flavor or image of servers want to create

10-run playbook
-create servers:
````
$ ansible-playbook sox_compliance/create.yml -i inventory/testgroup1/master/ -vvvv
````
-delete servers:
````
$ ansible-playbook sox_compliance/delete.yml -i inventory/testgroup1/master/ -vvvv
````
