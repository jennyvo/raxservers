# rax-servers
create, resize, delete servers

1-Spin up a vagrant box
````
$ mkdir vagrant_test //or what ever name
$ cd vagrant_test
$ vagrant init ubuntu/trusty64;
$ vagrant up
$ vagrant ssh
````
Note: edit VagrantFile to sync source code from local machine
````
# example add line to sync source to vm
config.vm.synced_folder "~/rax-servers/src/", "/home/vagrant/sox/"
````
2-create ansible_hosts
````
$ echo "127.0.0.1" > ~/ansible_hosts
$ export ANSIBLE_HOSTS=~/ansible_hosts
$ export ANSIBLE_HOST_KEY_CHECKING=false
````
3-update ~/.raxpub (template in src) folder with user credential 

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

10-run playbook:

  create servers:
````
$ ansible-playbook sox_compliance/create.yml -i inventory/testgroup1/master/ -vvvv
````
  resize servers:
````
$ ansible-playbook sox_compliance/update.yml -i inventory/testgroup1/master/ -vvvv
```` 
  delete servers:
````
$ ansible-playbook sox_compliance/delete.yml -i inventory/testgroup1/master/ -vvvv
````
***Because ansible adds keys to known_hosts, and Rackspace rotates IPs, you might come across ip contentions. If so, just clean out your known_hosts file.

> ~/.ssh/known_hosts
