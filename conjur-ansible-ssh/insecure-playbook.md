

Here comes a typical ansible playbook & inventory

### Inventory file 

First, let's review inventory file about our servers

`cat insecure-playbook/inventory`{{execute}}

The file should something like this:
```
[demo_servers]
host01 ansible_connection=ssh ansible_host=[[HOST_IP]] ansible_ssh_user=service01 ansible_ssh_pass=W/4m=cS6QSZSc*nd
host02 ansible_connection=ssh ansible_host=[[HOST2_IP]] ansible_ssh_user=service02 ansible_ssh_pass=5;LF+J4Rfqds:DZ8 
```

### Inventory file 

Next, the playbook for the 2 hosts

`cat insecure-playbook/insecure-playbook.yml`{{execute}}


### Let's try the sample playbook

```
export ANSIBLE_HOST_KEY_CHECKING=False
ansible-playbook -i insecure-playbook/inventory insecure-playbook/insecure-playbook.yml
```{{execute}}
