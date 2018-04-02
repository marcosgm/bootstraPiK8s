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

In the "secrets.yaml" file we'll specify the new password you want for the 'pirate' username (which is sudoer in Hypriot OS), as well as the ISCSI targets we want to use for /mnt folder later used for Docker storage
```
pirate_password: PASSWORDHERE
iscsi_portal: 192.168.2.253
iscsi_target:
  raspb2M: iqn.2004-04.com.qnap:ts-431p:iscsi.raspb2m.139e05
  raspb3A: iqn.2004-04.com.qnap:ts-431p:iscsi.raspb3a.139e05
  raspb3B: iqn.2004-04.com.qnap:ts-431p:iscsi.raspb3b.139e05
  raspb3C: iqn.2004-04.com.qnap:ts-431p:iscsi.raspb3c.139e05
```


# Launching the playbook
execute in order:
```
ansible-playbook -i raspinventory firstboot.yml
ansible-playbook -i raspinventory setupiscsi.yml 
ansible-playbook -i raspinventory installk8s.yml 
```
