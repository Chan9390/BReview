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
