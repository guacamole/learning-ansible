# Day 1 - Getting started with Ansible

## Installing Ansible on Windows10

1. Install WSL using the following command,run powerShell as admin:

```markdown
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

 2. Follow the instructions [here](https://docs.microsoft.com/en-us/windows/wsl/install-win10#install-your-linux-distribution-of-choice) for installing linux version on your windows machine.

 3. Install pip and Ansible on your system

```markdown
sudo apt-get -y install python-pip python-dev libffi-dev libssl-dev
pip install ansible --user
```

 

4. Create a directory using and two files inside it:

```markdown
mkdir ansible
cd ansible
touch boral-inventory ansible.cfg
```

5. Open Ansible directory in VS code and define contents of the above files:

```markdown
code .
```

6. Add the following content to **boral-inventory**

```markdown
[server-1] #Name of the group
<ipaddress>
```

7. Overwrite the default configuration in ansible.cfg

```markdown
[defaults]
INVENTORY = boral-inventory
```

8. Running the Ansible command to check the connectivity:

Username will be defined in the server

```markdown
ansible -i boral-inventory server-1 -m ping -u <linux server username>
```

9. Install a virtual Server in the system to play with: 

Now you might not have a real server to work/test ansible commands on, so create a virtual one on the local system with the help of [Vagrant](https://www.vagrantup.com/) and [Virtual box](https://www.virtualbox.org/wiki/Downloads)

Once both of these are downloaded and installed in the system, run following command on the terminal to initialize Vagrant.Now, you can provide an OS image of choice 

```markdown
vagrant init <OS image>
#geerlingguy/centos
```

This will give you a Vagrant file to work with a few commands in it. Open the Vagrant file and write following commands:

```markdown
config.vm.box = "geerlingguy/centos7"        # already there
  config.vm.provision "ansible" do |ansible| # to tell Vagrant do things Ansible way
    ansible.playbook = "playbook.yml"        # Referring to the playbook to carry out tasks
  end
end # already there 

```

10. Creating the playbook we referred to in the earlier point: 

```markdown
touch playbook.yml

name: All about NTP # name of the play 
  hosts: all        # hosts to perform the task on
  become: yes       # Becoming Sudo in order to be able to run admin commands later
  tasks:
    - name: Ensure NTP is Installed  #always a good practice to name the task
      yum: name=ntp state=present    #yum is a package manager to install/install and update softwares 
    - name: Ensure NTP is running
      service: name=ntpd state=started enabled=yes #service is used to enable/start/ru the S/W
```

11. Provisioning with Ansible: After the playbook has been created, we'll test if the tasks specified in it are being worked on

```markdown
vagrant up        #Start the VM
vagrant provision #run the specified tasks
```
