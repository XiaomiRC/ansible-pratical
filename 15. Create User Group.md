Write ```group.yml``` to create a user group on the nodes.
```
--- # Create a User group on nodes
- name: Create group
  hosts: host_group_name
  become: yes
  tasks:
    - name: Create group
      group:
        name: user_groupname
        state: present
```
