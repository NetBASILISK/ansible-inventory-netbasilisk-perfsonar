playbook for perfSONAR deployment and config

**Quick Start**:

Clone the playbook:

```
git clone https://github.com/perfsonar/ansible-playbook-perfsonar.git
cd ansible-playbook-perfsonar
```

Get the required roles (note that we ignore errors so we can run this multiple times):

```
ansible-galaxy install -r  requirements.yml --ignore-errors
```

Clone this inventory in the playbook dir.

```
git clone https://github.com/NetBASILISK/ansible-inventory-netbasilisk-perfsonar.git
```

Use Ansible ping to verify connectivity to targets:

```
ansible all \
  --ask-pass --ask-become-pass \
  -i ansible-inventory-netbasilisk-perfsonar/inventory \
  -m ping
```

Run the playbook:

```
ansible-playbook \
  --ask-pass --ask-become-pass \
  -i ansible-inventory-netbasilisk-perfsonar/inventory \
  perfsonar.yml
```

---
