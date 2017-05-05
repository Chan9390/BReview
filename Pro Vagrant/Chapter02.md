## Four Web Frameworks in Four Minutes

- Covers the frameworks:
  - AngularJS (JavaScript)
  - Django (Python)
  - Rails (Ruby)
  - Symfony (PHP)
- In the example, we are going to use the following ports:
  - AngularJS project : port 8800
  - Django project : port 8000
  - Ruby on Rails project : port 3000
  - Symfony project : port 8880

#### All the code examples are present in [https://github.com/pro-vagrant](https://github.com/pro-vagrant)

- You can list all the VMs using the command - `vagrant global-status`

#### This chapter does nothing more that cloning all the 4 repos and using `vagrant up` (with some explanation)

### Shared Folders

- By default, the directory in which you run `vagrant up` command is available inside the guest OS i.e. *both the guest and host OS share the project's directory*

### Stopping VMs

- You can change to the directory which the vagrant file is present and then execute `vagrant destroy`
- Or you could see the list of running VMs using `vagrant global-status`, and destroy the VMs anywhere using the syntax `vagrant destroy <ID>`.

> If you have shutdown the computer without destroying the VMs, the `vagrant global-status` might contain stale entries. To remove them run, `vagrant global-status --prune`
