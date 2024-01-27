# ansible-pratical
Practice notes
Verify each managed node is able to be accessed by Ansible 
```ansible -i /etc/ansible/hosts private-ip-of-node -m ping```

Verify all nodes in the managed node group is able to be accessed by Ansible 
```ansible -i /etc/ansible/hosts node-group-name -m ping```

Redirect the output to a file call pingstatus.txt
```ansible -i /etc/ansible/hosts node-group-name -m ping > pingstatus.txt```
