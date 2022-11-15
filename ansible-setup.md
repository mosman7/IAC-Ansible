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
- Ansible playbooks are a .yml files written in YAML
- Yet Another Markup Language (YMAL)
- Playbooks start with --- 3 dashes
## Create playbooks to launch node app
1. In /etc/ansible create a yaml file to install nginx
    - `sudo nano nginx.yml`
2. create a script to configure nginx in our web server - nginx.yml
3. To run the playbook `sudo ansible-playbook nginx.yml`
4. check status of nginx- should be running `sudo ansible web -a "systemctl status nginx"`
5. To check if the syntax is correct run
    - syntax check `sudo ansible-playbook configure_nginx.yml --syntax-check`
6. Next we need to install Node and NPM - node.yml
    - Node must be v12
    - If you have an old version run `sudo apt-get purge nodejs`
    - Install PM2 
7. Copy app folder into controller vm from local host
    - copy line into vagrant file `controller.vm.synced_folder "/Users/moham/Documents/app", "/home/vagrant/app"`
8. Copy app from controller to web
    - `sudo ansible web -m copy -a "src=~/moham/Documents/app dest=/home/vagrant/"`
9. Run a shell script to cd into app folder 
    - Install NPM
    - Seed the database
    - Run PM2
#### Now we need to set up the DB
1. Install mongodb - mongo.yml
2. Next we need to delete the mongo.conf file and make a new one with the new bind IP
3. Save the .conf file
4.  Check status of mongodb
    - `sudo ansible db -a "systemctl status mongodb"`

## Now the 2 playbooks are set up we can run them
- `sudo ansible-playbook mongo.yml` - loads the db
- `sudo ansible-playbook node.yml` - launches app
- when all tests have passed 
    - go to web ip `192.168.33.10`
    - add `:3000` for node app
    - add `/posts` for database