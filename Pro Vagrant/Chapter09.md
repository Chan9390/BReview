## One True Workflow

### Generating Base Boxes

- Two ways:
  - Generate bare-bones base boxes with Packer
  - Customize bare-bones boxes with provisioners

#### Generating Base Boxes with Packer

- Make use of the repository https://github.com/boxcutter

#### Generating Customized Boxes

- To copy files from the host system to guest system: `config.vm.provision "file", source: "file_name.txt", destination: "file_name.txt"`
- To run inline shell scripts: `config.vm.provision "shell", inline: "<bash command>"`

### Modularize Boxes with Provisioner Modules

- Puppet : Modules, Chef : cookbooks, Ansible : playbooks
- Puppet modules: https://forge.puppetlabs.com
- Chef cookbooks: https://supermarket.chef.io
- Ansible playbooks: https://galaxy.ansible.com
- You can install puppet modules using: `sudo puppet module install puppetlabs-apache --version 1.4.0 --force`

##### Everything else in this chapter is regarding Puppet which is not explained throughly
