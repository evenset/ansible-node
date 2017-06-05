# ansible-node
Better automation stategy for deploying NodeJS / ExpressJS application in production

# Software requirements
* [Ansible](http://docs.ansible.com/ansible/intro_installation.html) is a must, otherwise, no automation can be done
* You need to install [Vagrant](https://www.vagrantup.com/docs/installation/) if you're planning to
test your set up locally.
* [VirtualBox](https://www.virtualbox.org/wiki/Downloads) to actually download Ubuntu 16.04 image to your host machine locally.


# Useful terminal commands
## Vagrant
**Setup the box for the first time**
```bash
vagrant up
```
**Connect (ssh) to the box**
```bash
vagrant ssh
```
**(Re)provision the box**
```bash
vagrant provision
```
**Restart the box**
```bash
vagrant reload
```
**Shutdown the box**
```bash
vagrant halt
```
**Completely delete the box**
```bash
vagrant destroy
```

# Common issues
## Vagrant is not reachable from host on Ubuntu 17.04
As per this [stackoverflow](https://stackoverflow.com/questions/43455777/cant-get-to-any-vagrant-sites-on-ubuntu-17-04)
link you should disable `127.0.1.1 [computer-name]` from `/etc/hosts/` and also install
```bash
sudo apt-get install net-tools
```





