# 0.1 Introduction
This GitHub project is part of the "DevOps Beginners to Advanced" course on Udemy. As part of the project, the following stack is used: Vagrant, Nginx, Tomcat, RabbitMQ, Memcached, and MySQL.

We are using this stack to create a website. It will be set up locally, automated and the entire infrastructure will be on a code.

## 0.1.1 Architecture design of the project.

This is the flow, 

![image](https://github.com/user-attachments/assets/d9490409-f168-4bc6-99da-f464c3d12783)

And each service and "aplication" will have is own VM:



# 0.2 Prerequisites
- A HyperVisor, in my case Oracle Virtual Box,
- Vagrant, to create the VMs in a "automate" way,
- Git Bash, for CLI (Command Line Interface),
- VSCode, for IDE (Integrated Development Environment).
  
# 1.0 Creating the Virtual Machines.

Firstly, we will be using Vagrant to create the Virtual Machines. Understanding how Vagrant works it is crucial, like:

**1. Creating a folder where we wil be storing our Virtual Machines.**

```
mkdir carpeta_nueva
```

**2. Initialize a new Vagrant environment by creating a Vagrantfile in the current directory:**

> [!TIP]
> We need to be in the folder we just made.
>
> And, I personally recommend checking if we have any other Virtual Machine turned on. And if >there is any, turn them off.
>```
>vagrant global-status
>```

```
vagrant up
```

Now, let's check what have we done so far:

```
ls -a
```

**And there has to be a "Vagrantfile"**, then edit it with whatever text editor you want, I used VSCode. And **paste this entire configuration**:

```
Vagrant.configure("2") do |config|
  config.hostmanager.enabled = true 
  config.hostmanager.manage_host = true
  
### DB vm  ####
  config.vm.define "db01" do |db01|
    db01.vm.box = "eurolinux-vagrant/centos-stream-9"
    db01.vm.box_version = "9.0.43"
    db01.vm.hostname = "db01"
    db01.vm.network "private_network", ip: "192.168.56.15"
    db01.vm.provider "virtualbox" do |vb|
     vb.name = "DatabaseServer01"
     vb.memory = "600"
   end
  end
  
### Memcache vm  #### 
  config.vm.define "mc01" do |mc01|
    mc01.vm.box = "eurolinux-vagrant/centos-stream-9"
    mc01.vm.box_version = "9.0.43"
    mc01.vm.hostname = "mc01"
    mc01.vm.network "private_network", ip: "192.168.56.14"
    mc01.vm.provider "virtualbox" do |vb|
     vb.memory = "600"
   end
  end
  
### RabbitMQ vm  ####
  config.vm.define "rmq01" do |rmq01|
    rmq01.vm.box = "eurolinux-vagrant/centos-stream-9"
    rmq01.vm.box_version = "9.0.43"
    rmq01.vm.hostname = "rmq01"
    rmq01.vm.network "private_network", ip: "192.168.56.13"
    rmq01.vm.provider "virtualbox" do |vb|
     vb.memory = "600"
   end
  end
  
### tomcat vm ###
   config.vm.define "app01" do |app01|
    app01.vm.box = "eurolinux-vagrant/centos-stream-9"
    app01.vm.box_version = "9.0.43"
    app01.vm.hostname = "app01"
    app01.vm.network "private_network", ip: "192.168.56.12"
    app01.vm.provider "virtualbox" do |vb|
     vb.memory = "800"
   end
   end
   
  
### Nginx VM ###
  config.vm.define "web01" do |web01|
    web01.vm.box = "ubuntu/jammy64"
    web01.vm.hostname = "web01"
  web01.vm.network "private_network", ip: "192.168.56.11"
  web01.vm.provider "virtualbox" do |vb|
     vb.gui = true
     vb.memory = "800"
   end
end
  
end
```
