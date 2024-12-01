ssh 52.200.231.186 

ansible server:
ssh-keygen > copy the key id_rsa.pub

asible client:
ssh-keygen > cd /root/.ssh/ > paste the copied key to authorized_keys file

now test ssh 52.200.231.186 from ansible server machine

## Ansible ad-hoc commands
these are used for simpler tasks

#commands:
ansible -i inventory 52.200.231.186 -m "shell" -a "touch ansible.txt"
ansible -i inventory all -m "shell" -a "touch ansible.txt"

-m stands for module
-a used for passing the shell command


## Ansible playbooks
used for complex tasks

##inventory
list of target servers
