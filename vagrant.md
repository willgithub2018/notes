centos desktop
mobile/m0bile

centos7org
vagrant/vagrant

## PowerShell
```
netsh winhttp show proxy
netsh winhttp set proxy "204.40.194.129:3128"
Invoke-WebRequest http://woshub.com
```

set http_proxy=http://x:x@proxy2.gonet.gov.on.ca:3128/  
set https_proxy=http://x:x@proxy2.gonet.gov.on.ca:3128/  
204.40.194.129  
204.40.194.129  
export http_proxy=http://x:x@204.40.194.129:3128/  
export https_proxy=http://x:x@204.40.194.129:3128/  

```
Usage: vagrant [options] <command> [<args>]

    -v, --version                    Print the version and exit.
    -h, --help                       Print this help.

Common commands:
     box             manages boxes: installation, removal, etc.
     cloud           manages everything related to Vagrant Cloud
     destroy         stops and deletes all traces of the vagrant machine
     global-status   outputs status Vagrant environments for this user
     halt            stops the vagrant machine
     help            shows the help for a subcommand
     init            initializes a new Vagrant environment by creating a Vagrantfile
     login
     package         packages a running vagrant environment into a box
     plugin          manages plugins: install, uninstall, update, etc.
     port            displays information about guest port mappings
     powershell      connects to machine via powershell remoting
     provision       provisions the vagrant machine
     push            deploys code in this environment to a configured destination
     rdp             connects to machine via RDP
     reload          restarts vagrant machine, loads new Vagrantfile configuration
     resume          resume a suspended vagrant machine
     snapshot        manages snapshots: saving, restoring, etc.
     ssh             connects to machine via SSH
     ssh-config      outputs OpenSSH valid configuration to connect to the machine
     status          outputs status of the vagrant machine
     suspend         suspends the machine
     up              starts and provisions the vagrant environment
     upload          upload to machine via communicator
     validate        validates the Vagrantfile
     version         prints current and latest Vagrant version
     winrm           executes commands on a machine via WinRM
     winrm-config    outputs WinRM configuration to connect to the machine
``` 
[vagrant centos desktop] (https://codingbee.net/vagrant/vagrant-enabling-a-centos-vms-gui-mode)
```
vagrant init centos/7
config.vm.provider "virtualbox" do |v|
  v.gui = true
  v.memory = 2048
  v.cpus = 2
end
```
## Ubuntu
```
config.vm.provider :virtualbox do |vb|
  vb.gui = true
end

vagrant ssh

sudo apt-get install xfce4
sudo startxfce4&

vagrant box update
```

## Enable desktop
```
vagrant ssh
sudo -i 
yum groupinstall -y 'gnome desktop'
yum install -y 'xorg*'
yum remove -y initial-setup initial-setup-gui
```

```
#Install Google Chrome

wget https://dl.google.com/linux/direct/google-chrome-stable_current_x86_64.rpm
sudo yum -y localinstall google-chrome-stable_current_x86_64.rpm

wget https://dl.google.com/linux/direct/google-chrome-stable_current_x86_64.rpm
sudo yum install ./google-chrome-stable_current_*.rpm
google-chrome &

```

## reboot vagrant
```
vagrant halt ; vagrant up
```
[Markdown cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet#links)
Inline `code` has `back-ticks around` it
| Tables        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 |


sudo tee /etc/yum.repos.d/vscode.repo <<ADDREPO
[code]
name=Visual Studio Code
baseurl=https://packages.microsoft.com/yumrepos/vscode
enabled=1
gpgcheck=1
gpgkey=https://packages.microsoft.com/keys/microsoft.asc
ADDREPO


/etc/environment or /root/.bashrc
export http_proxy=http://x:x@204....:3128
export https_proxy=http://x:x@204....:3128
export ftp_proxy=http://x:x@204....:3128
cat /etc/environment

https://www.googletagmanager.com/gtm.js?id=GTM-MPF56HM
https://www.googletagmanager.com/gtm.js?id=GTM-T7V5LF
