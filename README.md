# ansible-node
Better automation stategy for deploying NodeJS / ExpressJS application in production

# Software requirements
## Packages
* [Ansible](http://docs.ansible.com/ansible/intro_installation.html) is a must, otherwise, no automation can be done
* You need to install [Vagrant](https://www.vagrantup.com/docs/installation/) if you're planning to
test your set up locally.
* [VirtualBox](https://www.virtualbox.org/wiki/Downloads) to actually download Ubuntu 16.04 image to your host machine locally.
## SSH forwarding for git repository
There are two ways to clone the remote repo into your server or Vagrant box. Either use username and password
in the URL, which imposes security risks and attacker can get a hold on your username and password, or forward 
your host machine SSH agent to the box or your server. 

### Prepare SSH Agent Forwarding [Very important]

We use SSH agent forwarding to authenticate with GitLab when cloning the Git repositories on the
Vagrant boxes. If you want to learn about this, read about the "SSH agent" and "SSH agent forwarding."

#### Enable SSH Agent Forwarding on Your Development Machine

Follow these steps to set up agent forwarding:

1. If you're on linux ,uninstall the GNOME Keyring: `sudo apt-get autoremove gnome-keyring`
1. Sign out from your user account and back in.
1. Make sure you have an SSH key (it would be in the `~/.ssh` directory). If not, generate one by
   running this comand: `ssh-keygen -t ecdsa`. Remember to upload your *public* key to your Git provider
   too(Gitlab, Github, Bitcbuket).
1. Enable SSH agent forwarding for the Vagrant host. Add this to `~/.ssh/config`:
   ```
   ForwardAgent no

   Host 192.168.20.*
       ForwardAgent yes
   ```
1. Check permissions on your SSH config file. Run `chmod g=-,o=- ~/.ssh/config`.
1. Check whether your SSH agent is running (`ps aux | grep ssh-agent`). If it's not running, start
   it by running `ssh-agent`.
1. Check whether the SSH agent knows about your SSH key (`ssh-add -l`). If the agent has no identities,
   add your SSH key (`ssh-add ~/.ssh/your_ssh_key`).

Now the Vagrant box will be able to use your SSH key when cloning the app.


#### Configure your host to connect to Git provider
Make sure you add the identity

```
ssh-add ~/.ssh/id_rsa
```

Check the config file
```
nano ~/.ssh/config
```
You should have these:
```
Host git.evenset.com
  IdentityFile ~/.ssh/id_rsa
  User hesamd
  ForwardAgent yes
```

Vagrant file should have these:
```
Vagrant.configure("2") do |config|
  config.ssh.forward_agent = true
```
and also this in provision:
```
ansible.extra_vars = { ansible_ssh_user: 'ubuntu' }
```

test the SSH
```
vagrant ssh
vagrant@ubuntu-xenial:~$ ssh -T git@gitlab.com
Welcome to GitLab, John Doe!
```

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





