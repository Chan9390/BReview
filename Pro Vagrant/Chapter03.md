## The States of VM

**Get the source code of the Sinatra App Example at [https://github.com/pro-vagrant](https://github.com/pro-vagrant)**

### VagrantFile

- This file is written in Ruby language and contains the name of the base box for guest OS
- There might be different versions of vagrants, but all of them are backward-compatible i.e. VagrantFile of earlier version of Vagrant will work on all versions

Example:

```ruby
Vagrant.configure(2) do |config|
  config.vm.box = "http://boxes.gajdaw.pl/sinatra/sinatra-v1.0.0.box"
  config.vm.network :forwarded_port, guest: 4567, host: 45670, host_ip: "127.0.0.1"
end
```

**`Vagrant.configure(2)`** - uses version 2 of the VagrantFile i.e. the file works well in vagrant versions 1.x.x to 2.x.x. This could also be done with variable `VAGRANTFILE_API_VERSION = "2"` and passed as a parameter to the vagrant.configure().
```ruby
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # code
end
```

**`config.vm.box = "http://boxes.gajdaw.pl/sinatra/sinatra-v1.0.0.box"`** - sets the basebox to the one available at the url

**`config.vm.network :forwarded_port, guest: 4567, host: 45670, host_ip: "127.0.0.1"`** - defines port forwarding, i.e. the requests to port 45670 in host will be forwarded to 4567 in guest OS.

**Due to `host_ip` parameter, the guest OS will only accept connections originating at the host with the IP address set to 127.0.0.1. This will restrict external access to the VM.**

### Where does the VM come from ?

- Complete VM used by vagrant to create and run guest OS - stored in single file - called *box file* or *image file* as they are preinstalled OS
- These files generally have `.box` extension

### Booting the VM

- Process of booting contains 3 main stages
  - Stage 1 : Downloading and installing the box
  - Stage 2 : Importing the base box to the project
  - Stage 3 : Booting the system

#### Stage 1 : Downloading and installing the box

- During 1st stage, vagrant checks if the box is present in the system, if not, it downloads
- The name of the directory in which the box is stored is the same as the name / url given. But every `/` is replaced by `-VAGRANTSLASH-`
- Example: `https://something/s.box` will be stored in `https:-VAGRANTSLASH--VAGRANTSLASH-something-VAGRANTSLASH-s.box`

#### Stage 2 : Importing the base box to the project

- During this stage the box is copied from the Vagrant home folder to the VirtualBox folder `~/VirtualBox VMs/`
- One stored in VirtualBox folder is decompressed and has more size

#### Stage 3 : Booting the system

- After importing the system works in *headless mode* - i.e. no GUI
- `vagrant status` displays the status of the guest OS in current project
- When executing vagrant up, a folder `/.vagrant/` is created within the project folder which is created and destroyed when vagrant up and destroy. It also cant be copied
- *If you delete `/.vagrant/` folder, you will lose all the information about the VM*

### Files and Directories : Summary

- The files and directories considered:
  - Projects directory (place where VagrantFile is present) **(A)**
  - VM template directory (place where the box file is stored) **(B)**
  - VM instance directory (place in the VirtualBox folder where the decompressed running box is present) **(C)**

#### Files and Directories is Stage 1

- The directory denoted by B is created (if it doesnt exist)
- You can alter the directory using `VAGRANT_HOME` variable

#### Files and Directories in Stage 2

- Files from B is copied and uncompressed in C (C is created)

#### Files and Directories in Stage 3

- The guest OS boots up. Now `.vagrant` folder is created in the project directory

### Guest OS States

- 5 different states:
  - running
  - poweroff
  - saved
  - not created
  - aborted
- **running state :** The guest OS boots up normally. Its accessible via SSH, and other ports as per the config in the VagrantFile. Both directories B and C are created.
- **poweroff state :** The guest OS is powered off. Cant access the OS using SSH. Both B and C are created.
- **saved state :** The guest VM is frozen. The contents of RAM is copied to file in directory C.
- **not created state :** This state before `vagrant up` and after `vagrant destroy`. In first case B doesnt exist but in second case B exists but C doesnt exist
- **aborted state :** This state if the host machine has gone to sleep or Ctrl+C interrupt was pressed during `vagrant up`

### Vagrant Commands

- Some commands:
  - `vagrant status`
  - `vagrant global-status`
  - `vagrant up`
  - `vagrant halt`
  - `vagrant suspend`
  - `vagrant resume`
  - `vagrant destroy`
  - `vagrant reload`
- **`vagrant status`** : status of the VM created as per the VagrantFile in the folder. If no VagrantFile, raises error.
- **`vagrant global-status`** : status of all the VMs controlled by Vagrant
- **`vagrant up`** : no matter what the previous state was, you will end up with running state
- **`vagrant halt`** : stops the VM. This preserves all files in VM, but doesnt preserve the process or data in VM's RAM
- **`vagrant suspend`** : saves snapshot of the VM to the hard disk. All files, processes and VM's RAM content is preserved.
- **`vagrant destroy`** : VM instance directory - C is removed but not B
- **`vagrant reload`** : this command is used to reload changes in VagrantFile like port forwarding and shared directory
- **`vagrant ssh`** : allows you to access and control the VM via ssh

### How to Start and Stop a VM ?

- Three aspects to be considered:
  - time necessary to boot the system
  - space of hard disk necessary for storage
  - which resources - files, processes, VM's RAM content - to be preserved
- To reduce start up time, preserve the contents of VMs RAM, use `suspend`
- To reduce space of storage use `vagrant destroy` - all the files will be destroyed
- To preserve files alone use `vagrant halt`

### Colliding Ports

- If a forwarded port is already in use, either close the application that uses the port or change the port in VagrantFile
- If you have changed the port in VagrantFile after creating a VM, do `vagrant reload` for the changes to take place in the VM

### Removing the Box

- Use `vagrant box list` to see the list of boxes on the host machine
- Use `vagrant box remove <BOX NAME>` to remove the box file (the template)
