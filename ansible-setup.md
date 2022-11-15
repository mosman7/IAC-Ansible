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
`ansible all/<server-name> -m copy -a "src=/etc/ansible/hosts dest=/home/vagrant"` -> Sends a file over using the copy method 
- can also `.` to specify local directory

## Playbooks
1. In /etc/ansible create a yaml file
    - `sudo nano nginx.yml`
2. create a script to configure nginx in our web server- nginx.yml
3. To run the playbook `sudo ansible-playbook configure_nginx.yml`
4. check status of nginx- should be running `sudo ansible web -a "systemctl status nginx"`

- syntax check `sudo ansible-playbook configure_nginx.yml --syntax-check`

- Install Node and NPM - node.yml
- Copy app folder into controller vm from local host
- Copy app from controller to web
- `sudo ansible web -m copy -a "src=~/moham/Documents/app dest=/home/vagrant/"`
- Install mongodb - mongo.yml
- Check status of mongodb
    - `sudo ansible db -a "systemctl status mongodb"`