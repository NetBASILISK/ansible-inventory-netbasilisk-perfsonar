playbook for perfSONAR deployment and config

**Quick Start**:

You must have an account on each of the MiServers and be in the not2fa group

Clone the playbook:

```
git clone https://github.com/perfsonar/ansible-playbook-perfsonar.git
cd ansible-playbook-perfsonar
```

Clone this inventory in the playbook dir:

```
git clone https://github.com/NetBASILISK/ansible-inventory-netbasilisk-perfsonar.git
```

Get the required roles (ignore errors so we can run this multiple times):

```
ansible-galaxy install -r requirements.yml --ignore-errors
ansible-galaxy install \
  -r ansible-inventory-netbasilisk-perfsonar/requirements.yml\
  --ignore-errors
```

Use Ansible ping to verify connectivity to targets:

```
ansible all \
  --ask-pass --ask-become-pass \
  -i ansible-inventory-netbasilisk-perfsonar/inventory \
  -m ping
```

Run the account provisioning playbook for the netbasilisk-perfsonar group:

```
ansible-playbook \
  --ask-pass --ask-become-pass \
  -i ansible-inventory-netbasilisk-perfsonar/inventory \
  ansible-inventory-netbasilisk-perfsonar/playbooks/miserver.yml
```

Run the perfsonar provisioning playbook:

```
ansible-playbook \
  --ask-pass --ask-become-pass \
  -i ansible-inventory-netbasilisk-perfsonar/inventory \
  perfsonar.yml
```

**Management Commands:**

Deploy only MadDash

```
ansible-playbook \
  --ask-pass --ask-become-pass \
  --limit ps-maddash \
  -i ansible-inventory-netbasilisk-perfsonar/inventory \
  perfsonar.yml
```

Deploy only PWA

```
ansible-playbook \
  --ask-pass --ask-become-pass \
  --limit netbasilisk-pwa.miserver.it.umich.edu \
  --become --become-user root \
  --become-method sudo \
  -i ansible-inventory-netbasilisk-perfsonar/inventory \
  perfsonar.yml
```

Edit PWA users

```
vi ansible-inventory-netbasilisk-perfsonar/inventory/group_vars/all/perfsonar/ps_pwa.yml
```

Provision PWA Users only

```
ansible-playbook \
  --ask-pass --ask-become-pass \
  --limit netbasilisk-pwa.miserver.it.umich.edu \
  --tags ps::pwa_users \
  -i ansible-inventory-netbasilisk-perfsonar/inventory \
  perfsonar.yml
```

Display auth interfaces on Archivers:

```
ansible ps-archives \
  --ask-pass --ask-become-pass \
  --become-user root --become \
  -i ansible-inventory-netbasilisk-perfsonar/inventory \
  -a "/usr/sbin/esmond_manage list_user_ip_address"
```

Delete an auth interface on Archivers:

```
ansible ps-archives \
  --ask-pass --ask-become-pass \
  -i ansible-inventory-netbasilisk-perfsonar/inventory \
  -a "/usr/sbin/esmond_manage delete_user_ip_address USERNAME IPADDR"
```

Invoke AGLT2 testing

```
ansible-playbook \
  --ask-pass \
  -i ansible-inventory-netbasilisk-perfsonar/inventory \
  --extra-vars "test_script=aglt2 test_loop_count=2" \
  ansible-inventory-netbasilisk-perfsonar/playbooks/testing_loop.yml
```
