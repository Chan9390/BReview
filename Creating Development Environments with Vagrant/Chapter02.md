## Chapter 2: Managing Vagrant Boxes and Projects

### Creating our first Vagrant project

- Initialize vagrant using - `vagrant init`
- Use `vagrant init --force <box_name>` to replace the existing Vagrantfile.
- Boxes are minimal installations of operation systems

### Managing Vagrant-controlled Guest machines

#### Powering up a Vagrant-controlled virtual machine

- When you do `vagrant up` the following things happen:
  - Copies the base box
  - Create a virtual box with the relevant provider
  - Forward any configured ports. By default, forwards port 22 to 2222
  - Boot the virtual machine
  - Configure and enable networking
  - Map shared folders between the host and guest. By default, maps folder containing Vagrant project to `/vagrant` on guest machine
  - Run any provisioning tools like Shell, Puppet, Ansible and Chef.

#### Suspending a virtual machine

- Execute `vagrant suspend`

#### Resuming a virtual machine

- Execute `vagrant resume`

#### Shutting down a virtual machine

- Execute `vagrant halt`

#### Starting from Scratch

- You can delete the virtual machine using `vagrant destroy`

#### Updating based on Vagrantfile changes

- Execute `vagrant reload`

#### Connecting to the virtual machine over SSH

- Execute `vagrant ssh`
- In Windows, ssh is not available. So use external SSH clients like: `PuTTY`.

### Managing integration between host and guest machines

#### Port Forwarding

- Use `config.vm.network :forwarded_port, guest: 80, host: 8888`
- To prevent conflicts, you can include `auto_correct: true` to the above command

#### Synced Folder

- Shares folders between host and guest
- Use `config.vm.synced_folder "/path/of/base/machine", "/path/in/VM"`
- To override the default synced folder: `config.vm.synced)folder ".", "/var/another/folder"`
- To enable NFS file transfers, include `type: "nfs"` i.e: `config.vm.synced_folder ".", "/var/another/folder", type: "nfs"`

### Networking

- Execute `config.vm.network "private_network", ip: "10.11.100.200"`
- To assign IP address via DHCP, execute `config.vm.network "private_network", type: "dhcp"`

### Autorunning commands

- Vagrant enables provisioning - to install and configure the machine
- Provisioning agents:
  - Shell
  - Puppet
  - Ansible
  - Chef
- Other supported provisioning tools:
  - Salt
  - Docker
  - CFEngine
- You can run shell provisioning in two ways:
  - Inline commands: `config.vm.provision "shell", inline: "sudo apt-get update"`
  - Provide shell script path: `config.vm.provision "shell", path: "script.sh"` (script.sh is present in the /vagrant folder)

### Managing Vagrant Boxes

- Use `--help` flag to know more about the command. Ex: `vagrant box --help`
- Vagrant box commands:
  - `add` : Adds a new box
  - `list` : Lists all existing boxes
  - `outdated` : Checks if any box has updates
  - `remove` : Removes the box
  - `repackage` : Converts Vagrant environment into a distributable box
  - `update` : Will update the box being used by current running Vagrant environment

#### Adding Vagrant Box

- Execute `vagrant add` along with the box
- It takes single arguement with some flags - the arguement is a name, URL or path to the box file
- `--force` : will tell vagrant to remove a pre-existing box with same name
- `--clean` : will tell vagrant to clean any temporary downloaded files
- `--provider` : will tell vagrant which virtualization provider to use at the backend. (VirtualBox is default)
- To add a new box : `vagrant box add --force packt http://some-server.com/packt.box`

#### Listing Vagrant Boxes

- Execute : `vagrant box list`

#### Checking for updates

- Execute : `vagrant box outdated`

#### Removing Vagrant Boxes

- Execute : `vagrant box remove <box_name>`
- `--provider` : to specify the provider
- `--box-version` : to specify which version of the box to delete
- Ex: `vagrant box remove hashicorp/precise64 --provider virtualbox`

#### Repacking a Vagrant Box

- `repackage` subcommand lets you convert Vagrant environment into a box that we can redistribute

#### Updating the current environment's box

- Execute: `vagrant box update`
- Use `--box` and `--provider` flag to be more specific.

#### Too many Vagrants !

- Execute: `vagrant global-status` to know the status of all the vagrant machines running
- You can use the ID of the machine to suspend them. Ex: `vagrant suspend a1b2c3d4`
