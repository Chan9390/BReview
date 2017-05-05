## Getting Started with Vagrant

### What is Vagrant ?

- Tool to simplify workflow and reduce workload to run and operate VMs.
- Also does the following:
  - Offers simple CLI to manage VMs
  - Supports major virtual solutions: VirtualBox, VMware, Hyper-V
  - Supports popular software configuration tools: Ansible, Chef, Puppet and Salt
  - Allows procedures to distribute and share virtual environments
- Vagrant boots the VM with the command `vagrant up` and stops the VM with the command `vagrant halt`
- Traditional method of virtualization:
  - Download VM image
  - Start VM
  - Configure VM's shared directories and network interfaces
  - Maybe install software with VM
- Vagrant's method:
  - Download and install VM
  - Boot the VM
  - Configure resources: RAM, CPUs, network connection and shared folders
  - Install additional software within VM using tools such as Puppet, Chef, Ansible, Salt
- In other words, it is a tool the provides simple and unified CLI to work with VMs managed by well known virtualization solutions

### Client/Server Paradigm and its AfterMath

- Client : process that sends requests; Server: process responsible for responding
- If you are developing a web application, the code is divided into two parts:
  - code that runs on the client (front end code)
  - code that runs on the server (back end code)

### Traditional Approach of Setting up a Developer Environment
- Two approaches on setting up developer env:
  - both on same computer as different processes
  - client and server run on different computer
- In the second approach, of using two different systems, the applications source code is synchronized with the IDE
- The user would do the following in second approach:
  - Synchronize files with the remote server with the IDE
  - Access the source code remotely (SSH/vi)
  - Emulate local address with drive mapping
- There are some shortcoming when using the both the methods

### Virtualization to the Rescue

- Both the server side and the client side code could be tested on the same machine but on different VMs / having the server code on a different VM and using the host browser to see the client code
- host system: the original OS of the developers machine
- guest system: the VMs OS

### Enter the Vagrant

- Vagrant - originally authored by Mitchell Hashimoto - under MIT license
- Supports well known virtualization solutions like VMWare Fusion, VMWare Workstation, Hyper-V, VirtualBox
- The underlying virtualization solutions are called **providers**

### Vagrant Rulez

- By using Vagrant, you will have a sandboxed solution in which no ports are exposed to the outside world (ie restricted) by default. If a port should be accessible from outside it should be explicitly mentioned.

### Disadvantages of Vagrant

- *Efficiency* - the workstations used by the developers (and also the sysadmin) should be powerful enough to work with the chosen provider (the sysadmin's workstation should be even more powerful as the process involves creating, packaging and testing the VM)
- There are other issues with some combinations of providers and Host OS - the access time to shared directory of VMs in VirtualBox is quite slow

### Installing the Software

- The three packages covered:
  - Git
  - VirtualBox
  - Vagrant

#### Git

- distributed version control system
- *de facto* standard of open source projects
- To get the version of git use : `git --version`

#### VirtualBox

- Download at https://www.virtualbox.org

#### Vagrant

- Download at https://www.vagrantup.com/downloads.html
- Check vagrant version using: `vagrant --version` or `vagrant -v`
- *Vagrant doesnt work in Windows if the user account contains non-Latin letters. Make sure the account doesnt have any non-Latin letters*

### Basic Vagrant Configuration

- By default, vagrant stores boxed VMs in `~/.vagrant.d/` folder
- These boxes should be shared to all who would have same development environment
- Vagrant home directory can be redefined using the `$VAGRANT_HOME` env variable. In linux, do `export $VAGRANT_HOME=/some/shared/directory`

### Documentation

- Use built-in manual using `vagrant -h` or `vagrant --help`
- `-h` of `--help` tag could be attached to any command to know more about it.
- Ex: `vagrant box -h` or `vagrant box --help`
- Know more at https://docs.vagrantup.com/v2/getting-started/
