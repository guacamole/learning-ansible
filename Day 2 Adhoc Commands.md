# Day - 2 Adhoc Commands

Getting familiar with few basic commands/adhoc commands to work with

1. Start with setting up a system like following:

![Day%202%20Adhoc%20Commands%202a2bf96a950040b8a251e9904fd6513a/Untitled.png](Day%202%20Adhoc%20Commands%202a2bf96a950040b8a251e9904fd6513a/Untitled.png)

```markdown
vagrant init geerlingguy/centos   #initialize Vagrant
code .                            #open the file in the editor,can also nvim <filename>

config.ssh.insert_key = false     #not inserting the private key, local system you know

config.vm.synced_folder = ".","/vagrant",disbled: true #disabling synced folders in case it causes issues

config.vm.provider = virtualbox do |v|
v.memory = 256
v.linked_clone = true

# Application server 1
 config.vm.define "app1" do |app|
 app.vm.hostname = "orc-app1.test"
 app.vm.network :private_network, ip: "192.168.60.4"
 end 

# Application server 2
config.vm.define "app2" do |app| 
app.vm.hostname = "orc-app2.test" 
app.vm.network :private_network, ip: "192.168.60.5"
end

# Database server
config.vm.define "db" do |db| 
db.vm.hostname = "orc-db.test" 
db.vm.network :private_network, ip: "192.168.60.6" 32
end

```

2. Starting Vagrant to load this structure along with it:

```markdown
vagrant up
```

3. To connect to these servers, we'll now create an inventory file 

```markdown
touch inventory
#Application server
[app]
192.168.60.4
192.168.60.5

#Database server
[db]
192.168.60.6

#Super group/all servers
[multi:children]
app
db

#

```