# ansible-mock-exam

This repository consists of Ansible EX407 mock exam questions along with solutions. 

# Infrastructure

There exists *Vagrantfile* which will build infrastructure by making it up. It boots up with *ansible* package being installed.
Below are the information which helps you build your ansible inventory files

You can use your laptop/desktop as *ansible-control-node* and other *ansible-?* nodes as *ansible-managed-nodes*
Your *ansible-control-node* has complete passworless SSH access to your *ansible-managed-nodes*

```
ansible-control.hl.local – Desktop/Laptop
ansible2.hl.local – managed host (192.168.56.11)
ansible3.hl.local – managed host (192.168.56.12)
ansible4.hl.local – managed host (192.168.56.13)
ansible5.hl.local – managed host (192.168.56.14)
```
