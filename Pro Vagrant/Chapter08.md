## Configuring Virtual Machines

### VirtualBox related Configuration

#### VM Name

- To set VM name to 'my-fantastic-project', use the following code:
  ```ruby
  Vagrant.configure(2) do |config|
    config.vm.box = "ubuntu/trusty64"
    config.vm.provider "virtualbox" do |v|
      v.name = "my-fantastic-project"
    end
  end
  ```
- If there is a machine already with the same name, an error is raised
- If the VM was started without "vagrant up" command, you can package using the command: `vagrant package --base vm-name`

#### GUI Mode

- By default, VirtualBox starts in headless mode
- You can enable GUI mode via:
  ```ruby
  Vagrant.configure(2) do |config|
    config.vm.box = "ubuntu/trusty32"
    config.vm.provider "virtualbox" do |v|
      v.gui = true
    end
  end
  ```
- In case of problem with vagrant up, use the debug flag: `vagrant up --debug`

#### RAM

- You can assign the memory of the VM using the following command:
  ```ruby
  Vagrant.configure(2) do |config|
    config.vm.box = "ubuntu/trusty32"
    config.vm.provider "virtualbox" do |v|
      v.memory = 4096
    end
  end
  ```
- You can verify the RAM in the guest VM by executing the following commands (inside guest VM)
  ```shell
  free m
  cat /proc/meminfo
  sudo lshw
  ```
- By default, VirtualBox assigns 512 MB RAM for each VM
- Make sure that you have enough RAM for your host system to function smoothly

#### Number of CPUs

- Use the following commands:
  ```ruby
  Vagrant.configure(2) do |config|
    config.vm.box = "ubuntu/trusty32"
    config.vm.provider "virtualbox" do |v|
      v.cpus = 2
    end
  end
  ```

#### Other physical properties

- You can set other physical properties using `v.customize ["modifyvm", :id, "NAME", "VALUE"]` command.
  ```ruby
  Vagrant.configure(2) do |config|
    config.vm.box = "ubuntu/trusty32"
    config.vm.provider "virtualbox" do |v|
      v.customize ["modifyvm", :id, "--cpuexecutioncap", "50"]
    end
  end
  ```
- The above code will set processor's execution time to 50 percent

### General Settings

#### Hostname

- Set name of the guest VM using `config.vm.hostname`
  ```ruby
  Vagrant.configure(2) do |config|
    config.vm.box = "ubuntu/trusty64"
    config.vm.hostname = "test.testing.lh"
  end
  ```
- Inside your guest VM, type `hostname -f` to know the name of the guest VM
- The name is available only to your guest, not host. To enable on the host append the following `127.0.0.1 test.testing.lh` to `/etc/hosts`. In windows hosts file is located in `C:\Windows\System32\Drivers\etc\hosts`

#### Post-up Message

- `config.vm.post_up_message` can be used to display a message after the boot is finished
  ```ruby
  Vagrant.configure(2) do |config|
    config.vm.box = "ubuntu/trusty64"
    config.vm.post_up_message = "The application is available at http://www.example.com"
  end
  ```

#### Booting and Halting Timeouts

- Timeouts for both booting and halting the guest VM
- By default they are 300 and 60 seconds by default
  ```ruby
  config.vm.boot_timeout = 300
  config.vm.graceful_halt_timeout = 60
  ```
- Boot and halt timeouts:
  ```ruby
  Vagrant.configure(2) do |config|
    config.vm.box = "ubuntu/trusty64"
    config.vm.boot_timeout = 1
    config.vm.graceful_halt_timeout = 5
  end
  ```
- `boot_time` - time to wait for the boot up. If the VM doesnt bootup within the given time, it will timeout
- `graceful_halt_timeout` - the number of seconds Vagrant waits after issuing a halt signal before shutting the guest down

#### SSH

- Available settings:
  ```ruby
  config.ssh.username
  config.ssh.password
  config.ssh.host
  config.ssh.port
  config.ssh.private_key_path
  ```
- The above settings define username, password, host, port and private key path.
- One more important option is: `config.ssh.insert_key` - boolean value. If `true` replaces publicly known SSH keys with new randomly generated pair
- If you are packaging a new box, disable this new ssh key insertion. If not the box will not be accessible using the publicly known keys. Set `config.ssh.insert_key = false`

#### X11 Forwarding

- Runs X11 applications within a guest system with windows displayed by the host system.
- On OS X use XQuartz, On Windows use Xming
- X11 forwarding:
  ```ruby
  Vagrant.configure(2) do |config|
    config.vm.box = "ubuntu/trusty64"
    config.vm.network "private_network", ip: "192.168.50.4"
    config.vm.forward_x11 = true
  end
  ```
- After you start the machine, use `vagrant ssh`
- Install x11 apps or firefox. When you start the x11 apps or firefox in the VM, the application windows will be showed in the host machine.
- If you are working on Windows you need to set the enivornment variable:
  ```bash
  export DISPLAY="192.168.0.100:0.0"
  xeyes
  ```
- Replace "192.168.0.100" with the IP you find in the Xming/View log

#### Base Box

- Two important variables: `config.vm.box` and `config.vm.box_url`
- Three forms of `config.vm.box`:
  - `vendor/name` will be resolved to atlas.hashicorp.com URL
  - a valid URL, ex: `https://www.example.com/something.box`
  - name like `rails-v1`, name of the box installed in the system

#### Using Checksum for Boxes

- Two variables: `config.vm.box_download_checksum` and `config.vm.box_download_checksum_type`
  ```ruby
  Vagrant.configure(2) do |config|
    config.vm.box_download_checksum_type = "sha256"
    config.vm.box_download_checksum = "1234567890"
    config.vm.box = "https://someurl.com/somebox.box"
  end
  ```

### Networking

#### Port Forwarding

- Port forwarding example:  
  ```ruby
  Vagrant.configure(2) do |config|
    config.vm.box = "ubuntu/trusty64"
    config.vm.network :forwarded_port, guest: 80, host: 8800
  end
  ```
- Port Forwarding can be further customized with the following five parameters:
  - guest
  - guest_ip
  - host
  - host_ip
  - protocol
- Use this to forbid remote access: `host_ip: "127.0.0.1"`

#### Port Collision

- If a port is already in use, then vagrant cannot assign the same port for any other service in guest VM. Use autocorrection to avoid port collision  
  ```ruby
  Vagrant.configure(2) do |config|
    config.vm.box = "ubuntu/trusty64"
    config.vm.network :forwarded_port, guest: 80, host: 8888, auto_correct: true
  end
  ```

#### Private Networks

- **To provide the IP address manually, you have to use the address from the private space**
- Manual Configuration of Private Networking
  ```ruby
  Vagrant.configure(2) do |config|
    config.vm.box = "ubuntu/trusty64"
    config.vm.network "private_network", ip: "10.20.30.40"
  end
  ```
- Private Network Configured with DHCP
  ```ruby
  Vagrant.configure(2) do |config|
    config.vm.box = "ubuntu/trusty64"
    config.vm.network "private_network", type: "dhcp"
  end
  ```
- The network interfaces once created will not be destroyed even if the VMs get destroyed. So to remove the network interfaces use: `VBoxManage hostonlyif remove vboxnet0`
- You can view the DHCP servers using the command: `vboxmanage list dhcpservers`
- You can remove DHCP servers using: `VBoxManage dhcpserver remove --netname HostInterfaceNetworking-vboxnet0`

#### Public Networks

- Public Network:
  ```ruby

  Vagrant.configure(2) do |config|
    config.vm.box = "ubuntu/trusty64"
    config.vm.network "public_network"
  end
  ```
- In this, Vagrant doesnt start any new interfaces, it uses an existing one
- To avoid questions on which interface to use, use bridge parameter: `config.vm.network "public_network", bridge: 'en1: Wi-Fi (AirPort)'`
- If you dont want to use DHCP server, assign IP address using: `config.vm.network "public_network", ip: "192.168.0.100"`

### Synced Folders

- By default, the directory in host containing the Vagrantfile is synchronized with the `/vagrant` folder in the guest VM, i.e.
  ```ruby
  Vagrant.configure(2) do |config|
    config.vm.box = "ubuntu/trusty64"
    config.vm.synced_folder ".", "/vagrant"
  end
  ```
- Depending on the Host OS, you have to define the shared paths. Example:
  - On Windows: `config.vm.synced_folder "C:\\User\\somefolder, "/agrant"`
  - On Linux: `config.vm.synced_folder "/some/path", "/vagrant"`
- Synchronizing multiple folders
  ```ruby
  Vagrant.configure(2) do |config|
    config.vm.box = "ubuntu/trusty64"
    config.vm.synced_folder "./some/dir/first", "/first", create: true
    config.vm.synced_folder "./some/dir/second", "/second", create: true
  end
  ```

#### Synchronization Types

- Vagrant offers 4 synchronization methods:
  - Default synchronization supported by provider (bidirectional, slowest performance)
  - NFS (bidirectional, best performance)
  - rsync (one way, host-to-guest)
  - SMB (bidirectional, performance comparable with NFS)

#### Benchmarking Disk Operations

##### VirtualBox Shared Folders

- Default synchronization :
  ```ruby
  Vagrant.configure(2) do |config|
    config.vm.box = "ubuntu/trusty64"
  end
  ```
- *Dont run Vagrant-driven virtual environments residing on removable USB memory or network-mapped drives*

##### NFS (Fastest)

- Using NFS for synchronization  
  ```ruby
  Vagrant.configure(2) do |config|
    config.vm.box = "ubuntu/trusty64"
    config.vm.network "private_network", type: "dhcp"
    config.vm.synced_folder ".", "/vagrant", type: "nfs"
  end
  ```
- For NFS to work, you *need private networking*
- **NFS is not available to Windows hosts, Vagrant ignores `type: "nfs"` even if it is present in Vagrantfile. To circumvent this restriction use vagrant-winnfsd plugin**
- On Linux, you might need to install NFS and provide root passowrd at some point of time. But that could be avoided

##### rsync (One-time, One-way host-to-guest)

- One way synchronization mainly when both default and NFS synchronization fails
- rsync Synchronization
  ```ruby
  Vagrant.configure(2) do |config|
    config.vm.box = "ubuntu/trusty64"
    config.vm.synced_folder ".", "/vagrant", type: "rsync"
      rsync__exclude: ".git/"
  end
  ```
- rsync happens only once during the boot up. Vagrant doesnt monitor the host folder for any changes
- To change this behaviour, use `rsync_auto` parameter to set `true`
- On Windows, you have to install rsync.exe

##### Server Message Block (SMB)

- SMB Synchronization:
  ```ruby
  Vagrant.configure(2) do |config|
    config.vm.box = "ubuntu/trusty64"
    config.vm.synced_folder ".", "/vagrant", type: "smb"
  end
  ```
- To use SMB, you need administrator privileges

### Platform-Related Configuration

- Using the `ffi` library and the `if` instruction,  a different configuration could be used depending on the host OS.
- Host-dependent configuration
  ```ruby
  Vagrant.configure(2) do |config|
    config.vm.box = "ubuntu/trusty64"

    require 'ffi'
    if FFI::Platform::IS_WINDOWS
      print "\n\n ===> win\n\n"
      config.vm.synced_folder ".", "/vagrant", :nfs => false
      config.vm.network :forwarded_port, guest: 80, host: 8880
    else
      print "\n\n ===> not win\n\n"
      config.vm.network :private_network, ip: "192.168.0.100"
      config.vm.synced_folder ".", "/vagrant", :nfs => true
    end
  end
  ```
