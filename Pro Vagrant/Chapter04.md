## Default Configuration and Security Settings of the Guest VM

### Atlas

- The base box (template / preinstalled OS) can be either be created from scratch or downloaded from network.
- From version 1.6, vagrant uses cloud service named Atlas to download base boxes. https://atlas.hashicorp.com - huge catalog of boxes
- Each box has 2 parts:
  - vendor's name
  - name of the box
- Ex: `hashicorp/precise64` : hashicorp is the vendors name; the box is precise64 - a Ubuntu machine built for 64 bit hardware
- Some well-known companies: Hashicorp, Ubuntu, Puppet and Chef

### Initializing a New Project

- Use `vagrant init` - this creates a `VagrantFile` in the directory
- To use a particular box, for example `ubuntu/trusty32`, use `vagrant init -m ubuntu/trusty32`. This box will be mentioned in the VagrantFile under `config.vm.box`
- `-m` flag creates a minimal VagrantFile (without any comments)
- `-f` flag will allow vagrant to overwrite VagrantFile if it is present in the directory

#### Security Concern #1

- Downloading the box over the network is a security risk, as it could have been tampered to contain malicious software.
- Downloading from unknown sources is also a risk. It is a serious threat as it could compromise the entire system and network.
- **Solution :** download boxes from trusted sources / build the box from scratch. Download boxes that use a well-known RSA key pair.

### Booting the Guest OS

- You can remove box using `vagrant box remove ubuntu/trusty32` if you want
- When you do `vagrant up`, vagrant checks for the box. As it is not available locally, it downloads from atlas.
- Default configuration of VM
  - **One forwarded port:** Host port 2222 to guest port 22. **The access is restricted to IP address 127.0.0.1 (via host_ip), so it is not accessible remotely**
  - **One shared folder:** Project folder shared by both guest and host machines.

#### Security Concern #2

- By default only ssh port is exposed.
- To expose any other port, it should be explicitly mentioned
- To restrict access only to the local machine, use `host_ip` parameter in `:forwarded_port` rule

### Sharing a Project directory

- The project folder is mounted as `/vagrant` in the guest VM
- To explicitly share a folder use `config.vm.synced_folder [HOST-PATH], [GUEST-PATH]`
- Example:
  - In Windows: `config.vm.synced_folder "C:\\some\\folder", "/dir/in/guest"`
  - In Mac: `config.vm.synced_folder "/some/dir/host", "/some/dir/guest"`

### Working with SSH

- SSH - kind of remote terminal session
- Could directly ssh the VM using `vagrant ssh`. Generally port 2222 is forwarded to guest port 22.
- If multiple guest VMs are present, the conflict of colliding ports will be taken care by vagrant - assigns different ports to different VMs
- Configuration of SSH sessions can be checked by `vagrant ssh-config`
- To manually open ssh session use `ssh -p2222 vagrant@127.0.0.1` with username and password as `vagrant`

#### Security Concern #3

- The user `vagrant` can use `sudo` - means the user has administrative priv.
- By default, the forwarded ssh port is restricted to connection from host OS only (again via host_ip)
- To expose to outside network, use `config.vm.network :forwarded_port, guest: 22, host: 9999`
- If exposed to outside network, use `passwd` to change the password of the guest machine

### Using the authorized_keys File for SSH Authorization

- Two methods of authenticating via SSH:
  - username/password
  - public and private cryptographic keys
- If our public key is stored in `~/.ssh/authorized_keys` in the destination machine, then the machine could be access using our private key
- This method - public/private keys - is used for authentication by default.
- The key files are stored at:
  - private key stored at: `~/.vagrant.d/insecure_private_key` in host system
  - public key stored at: `/home/vagrant/.ssh/authorized_keys` in guest system
- To change RSA key pair, do the following:
  - generate new key pair using: `ssh-keygen -t rsa -C johnny@example.net -f johnny`
  - this creates `johnny` (private key) and `johnny.pub` (public key)
  - copy `johnny.pub` to the project folder : `cp johnny.pub /the/vagrant/folder`
  - in guest OS, copy the contents of the file to authorized_keys : `cat johnny.pub > ~/.ssh/authorized_keys`
  - **mention the private key file location in the vagrant file: `config.vm.private_key_path = "/path/to/key/file"`**
  - do `vagrant reload` to apply changes

#### Security Concern #4

- Publicly available boxes use the so called publicly-known private keys. The key is the same by default.
- Latest versions of vagrant reduces this risk of publicly known key-pair by randomly generating key files on the fly
> **While publishing a box, you should use the publicly-known key pair. If not, no one could use (login) your box. To disable ssh key substitution use: `config.vm.insert_key = false`**
