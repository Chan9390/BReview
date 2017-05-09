## Provisioning

- CMS : configuration management software
- Some configuration management tools:
  - Shell scripts
  - Puppet
  - Ansible
  - Chef

### Provisioners

- the process of automatic installation and configuration of software within guest OS during `vagrant up` is called provisioning and the tools to perform this operation are called provisioners
- Vagrant supports the following provisioners:
  - File
  - Shell scripts
  - Ansible
  - CFEngine
  - Chef
  - Docker
  - Puppet
  - Salt

### Configuring Provisioning

- Syntax:
  ```ruby
  Vagrant.configure(2) do |config|
    # code
    config.vm.provision "NAME-OF-PROVISIONER"
  end
  ```
- Replace "NAME-OF-PROVISIONER" with:
  - shell
  - puppet
  - ansible
  - chef-solo
- Some provisioners doesnt require obligatory parameters. Ex: puppet
- Other provisioners require atleast one. Ex: shell
- Example:
  ```ruby
  Vagrant.configure(2) do |config|
    # code
    config.vm.provision "shell", path: "script.sh"
  end
  ```

### Multiple Provisioners

- Multiple provisioners could be used in one project
- Example:
  ```ruby
  Vagrant.configure(2) do |config|
    # code
    config.vm.provision "shell", path: "script.sh"
    config.vm.provision "puppet"
    config.vm.provision "ansible", playbook: "playbook.yml"
  end
  ```

### When Does Provisioning Happen ?

- By default, provisioning is executed only in the first `vagrant up` command. After that, vagrant assumes that the provisioning was completed during the initial boot up, so the provisioning doesnt execute.
- You can change the default behaviour by:
  - `vagrant up --provision` : runs the provisioners even if they were already executed in the past
  - `vagrant up --no-provision` : turns of provisioners
- You can enable provisioners by type using `vagrant up --provision-with x,y,z` in which on `x`, `y` and `z` provisioners are executed
- Example: `vagrant up --provision-with shell`
- Another way: `vagrant provision`
- If you have already booted the system, and want to test the provisioning scripts, you can do that using `vagrant provision`
- Other commands:
  - `vagrant reload --no-provision`
  - `vagrant reload --provision`
  - `vagrant reload --provision-with shell`

### Versioning Boxes with git

- Install git and initialize it using the commands:
  - `git config --global user.name "Name"`
  - `git config --global user.email "email@address.com"`
- These settings are stored in `~/.gitconfig` file

### Jekyll Box with Shell Provisioners

- Use shell provisioning
- Inside the script:
  ```ruby
  Vagrant.configure(2) do |config|
    config.vm.box = "ubuntu/trusty32"
    config.vm.provision "shell", path: "script.sh"
  end
  ```
- Bash script `script.sh`:
  ```bash
  #!/usr/bin/env bash

  echo "Installing: nodejs, lynx, ruby and jekyll..."
  apt-get update -y >>/tmp/provision-script.log 2>&1
  apt-get install nodejs -y >>/tmp/provision-script.log 2>&1
  apt-get install lynx-cur -y >>/tmp/provision-script.log 2>&1
  apt-get install ruby1.9.1-dev -y >>/tmp/provision-script.log 2>&1
  gem install jekyll -y >>/tmp/provision-script.log 2>&1
  ```
- The `-y` in the commands is for automatic yes reply to the prompts
- The above script will install NodeJS, Lynx, Ruby and jekyll
- The output will be redirected to `/tmp/provision-script.log`
- The two characters `>>` redirects the output to the file. The `2>&1` part is responsible for redirecting errors (file descriptor 2) to the same file as standard output(&1).

### First Run of the Shell Provisioner

- You can do `vagrant up`
- The output contains one red warning *"stdin: is not tty"*. It is a minor issue and could be safely ignored.
- The time for booting depends on the time taken for software installation

### Versioning the Box Source Code

- Add `.vagrant` and `*.box` to `.gitignore` so that they are ignored during the version control
  ```bash
  echo .vagrant > .gitignore
  echo "*.box" >> .gitignore
  ```
- You can add and cmmmit using:
  ```bash
  git add -A
  git commit -m "Message"
  ```
- The first command is called the staging changes in git terminology
- You can create tags using the command: `git tag`
- Example: `git tag -a 0.1.0 -m "Release 0.1.0"`
- You can check the available versions using `git tag`
- To see the complete history type: `git log --oneline --decorate`

### Generating a Box

- You can generate the box using `vagrant package --output something.box`

### Using the Shell Provisioned Box

- You can add the generated box:
  - `vagrant box add something-v0.1.0 something.box`
  - `vagrant box add something-v0.1.0 /some/directory/soemthing.box`
- You can include the box in the code using:
  ```ruby
  Vagrant.configure(2) do |config|
    config.vm.box = "something-v0.1.0"
    # code
  end
  ```
- Instead of manually executing the commands `jekyll serve -H 0.0.0.0 --detach`, you can modify the VagrantFile script as:
  ```ruby
  Vagrant.configure(2) do |config|
    config.vm.box = "vagrant-jekyll-shell-v0.1.0"
    config.vm.network :forwarded_port, guest: 4000, host: 8100, host_ip: "127.0.0.1"
    $script = <<SCRIPT
    cd /vagrant
    jekyll serve -H 0.0.0.0 --detach

    SCRIPT
    config.vm.provision "shell", inline: $script, run: "always"
  end
  ```
- The part between <<SCRIPT and SCRIPT is the text assigned to `$script`

### Annoying "not a tty" Problem

- There may be a red line "stdin: is not a tty" - its because of a command `mesg n` included in `root/.profile` script in `trusty32`
- You can safely remove it from `root/.profile` script and the red message will vanish
- Remove the command using `sudo sed -i "mesg n/d" /root/.profile` and then run provision using `vagrant provision`

### Jekyll Box with the Puppet Provisioner

#### Box Source Code

- Run the following commands:
  ```
  cd folder/with/examples
  mkdir vagrant-jekyll-puppet
  ```
- Create two files: `VagrantFile` and `manifests/default.pp`
- Contents of VagrantFile:
  ```ruby
  Vagrant.configure(2) do |config|
    config.vm.box = "ubuntu/trusty32"
    config.vm.provision "puppet"
  end
  ```
- Puppet script in `default.pp`:
  ```
  exec { 'apt-get update':
    command => '/usr/bin/apt-get update -y'
  }
  package { 'nodejs':
    require => Exec['apt-get update']
  }
  package { 'lynx-cur':
    require => Exec['apt-get update']
  }
  package { 'ruby1.9.1-dev':
    require => Exec['apt-get update']
  }
  exec { 'Install Jekyll':
    command => '/usr/bin/gem install jekyll',
    require => Package['ruby1.9.1-dev']
  }
  ```
- Puppet manifests  consists of resources such as `exec` or `package`
- The commands execute in a random order, the `require` commands are generally executed before installing the package

#### First Run of the Puppet Provisioner

- Use `vagrant up` and it will be automatically provisioned
- While using puppet provisioner you might come across the red warning: `warning: could not retrieve fact fqdn` - which mmeans there is a problem getting a fully qualified domain name(FQDN) for machine. By default Puppet tries to assign hostname using FQDN, you could manage this issue by adding rule: `config.vm.hostname = "abc.example.net"`. This sets the hostname, and FQDN is not queried
- You can pacakge it using `vagrant package` and also import it using `vagrant box add something.box`

### Jekyll Box with the Chef Provisioner

#### Box Source Code

- There are two files: `VagrantFile` and `cookbooks/jekyll/recipes/default.rb`
- Contents of VagrantFile:
  ```ruby
  Vagrant.configure(2) do |config|
    config.vm.box = "ubuntu/trusty32"
    config.vm.provision "chef_solo" do |chef|
      chef.add_recipe "jekyll"
    end
  end
  ```
- `default.rb` content:
  ```
  execute 'apt-get update'
  package 'nodejs'
  package 'lynx-cur'
  package 'ruby1.9.1-dev'
  execute 'gem install jekyll'
  ```

### Jekyll Box with the Ansible Provisioner

- To install Ansible on OS X:
  ```bash
  brew update
  brew install ansible
  ```
- To install Ansible on Ubuntu:
  ```bash
  sudo apt-get install software-properties-common
  sudo apt-add-repository ppa:ansible/ansible
  sudo apt-get update
  sudo apt-get install ansible
  ```
- Two files: `VagrantFile` and `playbook.yml`
- `VagrantFile` content:
  ```ruby
  Vagrant.configure(2) do |config|
    config.vm.box = "ubuntu/trusty32"
    config.vm.provision "ansible", playbook: "playbook.yml"
  end
  ```
- `playbook.yml` content:
  ```
  - hosts: all
    sudo: true
    tasks:
      - name: Update apt
        apt: update_cache=yes
      - name: Install nodejs
        apt: name=nodejs state=present
      - name: Install lynx
        apt: name=lynx-cur state=present
      - name: Install ruby1.9.1-dev
        apt: name=ruby1.9.1-dev state=present
      - name: Install Jekyll
        shell: 'gem install jekyll'
  ```
- The playbook is written in YAML format

### Workflow

- Two diverse procedures:
  - The procedure to create the box is carried out by system engineers
  - The procedure to use the box is carried out by developers

**Shebang**:
