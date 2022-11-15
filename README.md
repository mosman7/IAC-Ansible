### What is Infrastructure as Code
Infrastructure as Code (IaC) is the managing and provisioning of infrastructure through code instead of through manual processes.
With IaC, configuration files are created that contain your infrastructure specifications, which makes it easier to edit distribute configurations.

### Benefits of Infrastructure as Code
IaC can help your organisation manage IT infrastructure needs while also improving consistency and reducing errors and manual configuration.

- Cost Reduction
- Increase in speed of deployments
- Reduce errors
- Improve infrastructure consistency
- Eliminate configuration drifit

### What is Configuration management
- Configuration management is part of provisioning. Basically, that's using a tool like Chef, Puppet or Ansible to configure our server. 
- Provisioning often implies it's the first time we do it. Configuration management usually happens repeatedly.

### What is Orchestration
- Orchestration means arranging or coordinating multiple systems. It's also used to mean 'running the same tasks on a bunch of servers at once, but not necessarily all of them'

### What is Ansible
![ansible](https://user-images.githubusercontent.com/115226294/201696502-b131b5da-ac3a-4655-8e0c-1fd59b965252.png)

Ansible is a simple IT automation engine that automates cloud provisioning, configuration management, application deployment, infra-service orchestration, and many other IT needs.

#### Benefits
- Free - it is Open-Source.
- Very simple to set up and use - no need to know a lot of code to use Ansible's playbooks.
- Powerful - It lets you model even highly complex IT workflows.
- Flexible - Can orchestrate the entire application environment no matter where it's deployed
- Agentless - Do not need to install any other software or firewall ports on the client systems you want to automate, also don't need to set up a seperate management structure.
- Efficient - You do not need to install any extra software, there's more room for application resources on your server.

### Blue Green deployment
![blue green](https://user-images.githubusercontent.com/115226294/201678726-9d3d8c2a-680d-44c6-ad3e-5b43f364a032.jpeg)

- A blue/green deployment is a deployment strategy in which you create two separate, but identical environments. One environment (blue) is running the current application version and one environment (green) is running the new application version.
- When the new environment (green) is ready and tested, traffic is redirected from blue to green. If there are any problems then we switch back to blue while we fix these.
- Reduces downtime

### What is inventory
- Ansible reads information about the machines you manage from the inventory.- In our case the hosts file in /etc/ansible/hosts
- Inventory is listed in the file which contains IP addresses, databases, and servers
### What are ansible roles
Ansible roles allow you to develop reusable automation components by grouping and encapsulating related automation artifacts, like configuration files, templates, tasks, and handlers. Because roles isolate these components, it's easier to reuse them and share them with other people.