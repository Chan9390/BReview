## Your First Box

### The Task at Hand

- The scenario is that you are going to build a box file with jekyll installed, and use the box for further development of the static website.
- You should:
  - Install all the required tools to the VM
  - Port forward so that the static website could be viewed using a browser in host machine
- The exercise will involve two different folders:
  - `~/first-box-factory/` - directory to produce boxed VM
  - `~/corporate-blog/` - directory for storing the contents for the blog

### Choosing a Base Box and Initializing a New Project

- Choose a development environment (base box file) and initialize the vagrant file
- Change to the working directory (`first-box-factory` in this case) and initialize VagrantFile
  ```shell
  cd ~/first-box-factory
  vagrant init -m ubuntu/trusty32
  vagrant up
  ```

### Installing necessary Software

- You need the following software:
  - NodeJS
  - Ruby
  - Jekyll
- You would need lynx - a web browser which runs on a terminal
- SSH into the box using `vagrant ssh`
- Use the following commands:
  ```shell
  sudo apt-get update -y
  sudo apt-get install nodejs -y
  sudo apt-get install lynx-cur -y
  sudo apt-get install ruby1.9.1-dev -y
  sudo gem install jekyll
  ```

### Generating a Box

- You can package the box using the command `vagrant package`
- Example: `vagrant package --output first-box-jekyll.box`
- When the command is run without any parameters, `package.box` is created

### Listing, Installing, and Removing Boxes

- Box management commands:
  - `vagrant box list` - lists the installed boxes
  - `vagrant box add` - adds a box
  - `vagrant box remove` - removes a box
- These commands could be executed in any folder, as they affect global vagrant installation
- Adding box takes 2 parameters: Name of the box and Address of the box
- Syntax of adding box: `vagrant box add [NAME] [ADDRESS]`
- The address of the box can take one of the forms:
  - Vendor/Name - (when this is used, a box is downloaded from atlas.hashicorp.com)
  - http://...
  - file:///...
  - A local filename
- Example: `vagrant box add first-box-jekyll first-box-jekyll.box`
- After adding the box, `vagrant list` should show the box name in the list
- To remove a box, use `vagrant box remove NAME`
- Syntax: `vagrant box remove NAME --provider PROVIDER --box-version VERSION`
- Example: `vagrant box remove abc --box-version 1.2.3`
- versioning only works for boxes that are hosted remotely

### Using the Box

- Use the following commands:
  ```shell
  cd first-box-factory
  vagrant init -m first-box-jekyll
  vagrant up
  vagrant ssh
  ```
- In the guest OS ssh session,
  ```shell
  cd /vagrant
  jekyll new -f .
  jekyll build
  jekyll serve -H 0.0.0.0 --detach
  ```
- To check if the website is working use lynx : `lynx 127.0.0.1:4000`
- The `-H` parameter defines IP addresses that are allowed to connect to it. 0.0.0.0 allows connection from any IP address but the vagrant doesnt expose this port to outside world.

### Forwarding Ports

- Modify the VagrantFile
  ```ruby
  Vagrant.configure(2) do |config|
    configure.vm.box = "first-box-jekyll"
    configure.vm.network :forwarded_port, guest: 4000, host: 8080, host_ip: "127.0.0.1"
  end
  ```
- To apply these changes do `vagrant reload`
- After that do `vagrant ssh` and manully start the jekyll server again using `jekyll serve -H 0.0.0.0 --detach`

### Advantages of Boxing

- development environmen could be reproduced using one command `vagrant up`
- Some scenarios:
  - You made some changes to guest OS, dont know how to revert them. **Solution**: `vagrant destroy` and `vagrant up`
  - You need to create another Jekyll website **Solution**: Change to the directory and then do a `vagrant init && vagrant up`

### Serving Boxes over Network

- You can upload the box file to a webserver at `https://somewebserver.com/some.box`. You can then include the line: `config.vm.box = "https://somewebserver.com/some.box"`
- You can also use mapped or shared directories by using: `config.vm.box = "/net/drive/mounted/some.box"`

### Securing your Boxes

- Two ways:
  - Change the Vagrant's user password
  - Change the SSH keys used for authentication
- Do this before the `vagrant package` command

### Repackaging Boxes

- From the above scenario, there are box files in 2 directories: in `~/first-box-jekyll` and in `~./vagrant.d/boxes/`
- You can delete the file in first-box-jekyll using `rm first-box-jekyll`
- If you want the box file back, you could create one from the installed box files with `vagrant box repackage` command
- Syntax: `vagrant box repackage NAME PROVIDER VERSION`
- Example: `vagrant box repackage first-box-jekyll virtualbox 0`
- `package.box` will be created in the folder
