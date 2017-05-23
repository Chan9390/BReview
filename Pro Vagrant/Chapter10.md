## Going Pro

### Multimachine Settings

- Vagrant supports multimachine configurations
- Configuration example : one for web server and one for databse server using a single vagrantfile
  ```ruby
  Vagrant.configure(2) do |config|
    config.vm.define "web" do |web|
      web.vm.box = "chef/centos-6.5-i386"
    end
    config.vm.define "db" do |db|
      db.vm.box = "ubuntu/trusty64"
    end
  end
  ```
- All configuration options are avaible to both the machines
  ```ruby
  Vagrant.configure(2) do |config|
    config.vm.define "web" do |web|
      web.vm.box = "chef/centos-6.5-i386"
      web.ssh.insert_key = false
      web.vm.provisioner "virtualbox" do |v|
        v.memory = 4096
      end
    end
    config.vm.define "db" do |db|
      db.vm.box = "ubuntu/trusty64"
    end
  end
  ```
- To access each box you could do - `vagrant up <boxname>` ex: `vagrant up db`, other features: `vagrant halt db`, `vagrant destroy db`, `vagrant ssh web`
- To set the machine primary use `primary` keyword
  ```ruby
  config.vm.define "web", primary: true do |web|
    #....
  end
  ```

### Debugging

- Use `--debug` flag to get verbose output. Ex: `vagrant up --debug`

#### Using vagrant Plug-ins

- Information about all the plugins are available at: https://github.com/mitchellh/vagrant/wiki/Available-Vagrant-Plugins

##### Vagrant-cachier Plug-in

- Available at https://fgrehm.viewdocs.io/vagrant-cachier
- Manages cache folder of various applications on a per-base box basis
- This cache is not stored in guest, instead it is transferred to the host system and is accessible via a shared directory.
- This is helpful, if you are creating and destroying a particular VM multiple times, or starting up multiple VMs at the same time

##### Vagrant-winnfsd Plug-in

- This plugin turns on the NFS type for shared folders on hosts running Windows

##### Vagrant-puppet-install Plug-in

- Installs puppet provisioner in the guest OS (use this if base box doesn't contain puppet)

##### Vagrant-omnibus Plug-in

- Installs Chef provisioner in the guest OS

##### Vagrant-hostupdater Plug-in

- Helps manage the host files on the host

##### Vagrant-vbguest Plug-in

- Updates the VirtualBox Guest Additions within the guest to match the VirtualBox version installed on the host

### Working with Atlas.Hashicorp.com

- You can create an account at https://atlas.hashicorp.com and upload the box file for free. But hashicorp charges for the data transfers which happens when others download your box

### Versioning Boxes

- You can and should version your boxes
- To update an existing box, try running `vagrant box update`
- You can use the `vm.box_version` to use a version within a range of specified versions. Ex:`config.vm.box_version = ">0.1,<0.3"` and `config.vm.box_version = "<2.0,>=1.0"`. If box versions dont match then you will get an error message
- **To remove a specific version of box:`vagrant box remove gajdaw/apache --box-version 0.3.2`**

### Sharing Your Environment

- Atlas service allows you to share the development environment running on your machine with your colleagues. Anyone can gain access to your application.

#### HTTP Shares

- HTTP Shares allows anyone to access the web-application running in your VM within a web browser
- All you need to do is:
  ```bash
  cd /path/to/required/directory
  vagrant up
  vagrant login
  # You have to login using your atlas credentials
  vagrant share
  # Now a message will be displayed along with the URL to access
  # After you are done with sharing, then press Ctrl+C
  ```
- To logout from the session type `vagrant login -logout`

#### SSH Shares

- SSH Shares allow others to SSH into your VM.
- All you need to do is:
  ```bash
  cd /path/to/required/directory
  vagrant up
  vagrant login
  # Enter Atlas credentials
  vagrant share -ssh
  # You will be asked to create a new password
  # A URL will be provided, say for example http://this-example-51.vagrantshare.com
  ```
- On the other side you have to do `vagrant connect --ssh this-example-51` and type in the password when asked
- To stop press Ctrl+C, vagrant login -logout, and vagrant destroy -f.
- If you want to start SSH Share without HTTP share at same time, use the `--disable-http` flag. Ex: `vagrant share --ssh --disable-http`
