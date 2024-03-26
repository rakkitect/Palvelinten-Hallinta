# Installing Vagrant

Hashicorp descripes Vagrant as such:

    Vagrant is the command line utility for managing the lifecycle of virtual machines.
    Isolate dependencies and their configuration within a single disposable and consistent environment.

Vagrant can be found from https://developer.hashicorp.com/vagrant/install?product_intent=vagrant.
I'm using a laptop with Windows 11 OS so I'm choosing Windows -> AMD64

![Vagrant installation file](https://github.com/rakkitect/Server-Management/blob/main/Images/vagrant_w11_install.png)

Vagrant Virtual machines are initialized by a command:

    vagrant init debian/bullseye64
    vagrant up
    vagrant ssh

Once this is done, you can use the virtual machine from Command Prompt or PowerShell:

![Vagrant in PowerShell](https://github.com/rakkitect/Server-Management/blob/main/Images/vagrant_init.png)
