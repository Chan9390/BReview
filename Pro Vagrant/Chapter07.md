## Creating Boxes from Scratch

### Packer

- Packer - to automate the creation of preinstalled VMs for various OSs and providers
- The whole process:
  - It downloads the ISO CD or DVD images of OS
  - runs installation and applies default configuration
  - when finishes, Packer uses provisioners to customize system
  - export the system
  - pack the exported OS into a single \*.box file
- This procedure can be used for following purposes:
  - To produce arbitrary systems - ex Ubuntu, Windows7
  - To create box to arbitrary providers - ex VirtualBox, VMware, Parallel

### Installing Packer

- Available at https://www.packer.io/downloads.html
- For OS X,
  ```bash
  brew tap homebrew/binary
  brew install packer
  ```
- Get the version info at `packer version`
- Packer caches downloaded files on per-project basis, and they are stored at `packer_cache`
- You can change the packer cache directory by changing the `PACKER_CACHE_DIR` env variable:
  ```bash
  PACKER_CACHE_DIR=/some/cache/dir
  export PACKER_CACHE_DIR
  ```
- If you want them to run during every shell session, put them in `~/.profile`

### Building Boxes with chef/bento Project

- Download the repo: `git clone https://github.com/chef/bento`
- Change to the directory: `cd bento/packer`
- Current directory contains JavaScript Object Notation (JSON) files for different providers and different base boxes
- Building a box consists of so many stages, so takes time
- To build Ubuntu 14.04 for VirtualBox provider: `packer build -only=virtualbox-iso ubuntu-14.04-i386.json`
- Once the box is built, it would be present in `/bento/builds` folder
- Other example: `packer build -only=vmware-iso fedora-21-i386.json`
- The boxes produced are bare - stripped of various tools - dont even contain provisioning tools

### Using the Box Generated with chef/bento

- Add the box from the builds folder:
  ```bash
  cd bento/builds/virtualbox
  vagrant box add bento-ubuntu opscode_something.box
  ```
- Then you can use `vagrant init -m bento-ubuntu` to include the box
- `cat /home/vagrant/.vbox_version` displays the version of VirtualBox to generate the box

### Building Boxes Using the boxcutter Project

- Boxcutter builds boxes that are equipped with Puppet, Chef and Salt provisioners
- **If you want to work on Windows, you have to install `make` program. Visit cygwin.com**
- To build Ubuntu 14.04 equipped with latest version of Puppet
- Do the following:
  ```bash
  git clone http://github.com/pro-vagrant/ubuntu.git
  cd ubuntu
  ```
- Create `Makefile.local` with contents:
  ```
  CM := puppet
  UPDATE := true
  ```
- You can then run the command: `make virtualbox/ubuntu1404-i386`
- The `CM` means *Configuration Manager* and can take values puppet, chef, salt, nocm.

### Using the Box Generated with the boxcutter Project

- The box will be stored in `box/virtualbox` folder
- Use `vagrant box add` to add the box

### How does Packer Work ?

- Packer contains couple subcommands
- `packer build TEMPLATE` - TEMPLATE is a JSON file and defines how to build a box
- Basic structure of Packer Template
  ```json
  {
    "variables": {
      "mirror" : "https://releases.ubuntu.com",
      ...
    },
    "builders": [
      {
        "type": "virtualbox-iso",
        ...
      },
      {
        "type": "vmware-iso",
        ...
      },
      {
        "type": "parallels-iso",
        ...
      }
    ],
    "provisioners": [
      {
        "scripts": [
          "scripts/ubuntu/update.sh",
          "scripts/common/sshd.sh",
          "scripts/ubuntu/networking.sh",
          ...
        ],
        "type":"shell"
      }
    ],
    "post-processors": [
      {
        "type": "vagrant",
        ...
      }
    ]
  }
  ```
- Order of the template:
  - Variables
  - Builders
  - Provisioners
  - Post-processors
- All boxes could be produced in parallel
- `iso_url` defines the url where the ISO file could be downloaded - if the file is not present in PACKER_CACHE_DIR folder, the ISO is downloaded
- `iso_checksum` and `iso_checksum_type` - define checksum of ISO for file's integrity
- `modifyvm` under `vboxmanage` entries modify the memory and CPU of the VM
- `disk_size` key defines the disk size
- Packer waits the amount of time defined with the `boot_wait` parameter.
- `boot_command` contains the commands to be executed when the `boot_wait` time expires

### Customizing Boxes Generated with chef/bento

- Add the shell scripts you would like to execte during the boot up under `scripts` in `provisioners`
- The user account is created in the `preseed.cfg` file

### VirtualBox Guest Additions

- *Mismatched version of VirtualBox Guest Additions* - if the box was built with different VirtualBox version
- The file `/home/vagrant/.vbox_version` gives the VirtualBox version used to build it
- You could also use `vbguest` plugin

### Creating a Box Manually

- `packer_cache` the ISO file gets downloaded - the filename is SHA256 hash of the URL used to download the file
- You can try manually installing the OS. If you want to package the box after you manually install the OS, you can execute `vagrant package --base <name-of-the-VM>`. Example `vagrant package --base manual-ubuntu`.

#### Using the boot command and preseed.cfg

- If you are planning to have an automatic install of the OS, you got to give provide some boot commands
- You have to start a simple HTTP server using PHP or Python using any one of the below commands:
  ```
  php -S 0.0.0.0:8080 &
  python -m SimpleHTTPServer 8080 &
  ```
- Please note the process ID of the command for future reference
- Use ifconfig to find out your IP address, so that you could be sure of the address of the created HTTP server
- The preseed.cfg will be hosted and then called from the boot command.
- These boot commands come by default in the json files from the bento project. Each OS has its own boot commands

### Working with the Open Virtualization Format (OVF)

- Packer can also deal with .ovf files
- Instead of providing the path for the ISO file, you could directly provide the path for the OVF file. Packer deals with it for you.
- Example
  ```
  "builders": [
    {
      "type": "virtualbox-ovf",
      "source:path" : "/path/to/vagranthome/.vagrant.d/boxes/box-name/14.04/virtualbox/box.ovf",
      ...
    }
  ],
  ```
