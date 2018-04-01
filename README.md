# References
Adapted from https://blog.hypriot.com/getting-started-with-docker-and-linux-on-the-raspberry-pi/
and https://blog.hypriot.com/post/setup-kubernetes-raspberry-pi-cluster/

# How to use this playbook
Just populate /etc/hosts with the hostnames and IPs of your Raspberry Pi's.
Hint: to discover them, execute "name -sP 192.x.x.x/24 | grep Raspberry -B 3" , of course with your LAN's IP network instead of x.x.x

Then, create an Inventory file in this folder. This is my example (note the use of VARS to write the username, password and "become" settings
```
# cat raspinventory 
[hypriot:children]
master
nodes

[hypriot:vars]
ansible_user=pirate
ansible_password=hypriot
ansible_become=yes

[master]
raspb2M

[nodes]
raspb3A
raspb3B
raspb3C
```

# Setting Environment Variables

Just create a "secrets.yaml" file, to specify the new password you want for the 'pirate' username (which is sudoer in Hypriot OS)
pirate_password: PASSWORDHERE

# Launching the playbook
execute
ansible-playbook -i raspinventory firstboot.yml -e secrets.yaml 


