## Set up ansible
1. ssh into the controller vm
```
sudo apt-get update && sudo apt-get upgrade -y 

sudo apt-get install software-properties-common

sudo apt-add-repository ppa:ansible/ansible

sudo apt-get update -y

sudo apt-get install ansible -y

sudo apt-get install tree -y
```
2. cd into /etc/ansible - should be hosts in here
    - here we can ssh into other vms using ip
`sudo ssh vagrant@192.168.33.11` - will ssh into db vm
    - now we need to set a password - default password is `vagrant`

3. Now we need to connect to the vms using the connector
    - cd to /etc/ansible/hosts
    - `sudo nano hosts`
    - add
    ```
    [web]
    web-private-ip ansible_connection=ssh ansible_ssh_user=vagrant ansible_ssh_pass=vagrant

    [web]
    192.168.33.10
    ```
    ```
    [db]
    db-private-ip ansible_connection=ssh ansible_ssh_user=vagrant ansible_ssh_pass=vagrant

    [db]
    192.168.33.11
    ```
4. Now if you ping the 2 vms from controller it should return `success` -   password must be set up first!!
    - run `sudo ansible -m ping web` will return that thus is a success `"ping": "pong"`

### Some commands you can use to check connection

- `sudo ansible all -m ping` - pings all hosts
- `sudo ansible web/db -m ping` - select on of the hosts and ping the specific host

### ADHOC COMMANDS
- `ansible all/<server-name> -a "uname -a"` -> -a Stands for arguements.
- `ansible all/<server-name> -a "date"` -> Gets the time of that server and timezone.
- `ansible all/<server-name> -a "free -m"` -> Shows how much memory is available
#### Moving files
`ansible all/<server-name> -m copy -a "src=file-path dest=destination-file-path"` -> Sends a file over using the copy method 
- can also `.` to specify local directory

Playbooks
in /etc/ansible create a yaml file
nginx.yml
#create a script to configure nginx in our web server   
```
# who is the host - name of the server
- hosts: web
# gather data
  gather_facts: yes
# we need admin access
  become: true
# add instructions
  tasks:
  - name: Install/configure Nginx web server in web-VM  
    apt: pkg=nginx state=present
# ensure nginx is running
```
to run the playbook `sudo ansible-playbook configure_nginx.yml`
check status `sudo ansible web -a "systemctl status nginx"`
