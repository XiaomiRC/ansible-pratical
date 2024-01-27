# ansible-pratical
Practice notes
## Establishing connection between Ansible Server and Nodes
1. Start 3 EC2 on AMI Amazon Linux 2. Name them Ansible Server, Node1 and Node2.
2.1 Download epel-release-latest-7 on Ansible Server.
   ```
   wget https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
   ```

2.2 Install the downloaded  epel-release-latest-7.noarch.rpm file on Ansible server.
```
sudo yum install epel-release-latest-7.noarch.rpm -y
```

3 Install the following packages on the Ansible Server:
```
yum install git openssl python python-level python-pip ansible -y
```

4.1 Create a user called 'ansible' on the Ansible Server, Node1 and Node2. Set a password for this user.
```
adduser ansible
```
```
passwd ansible
```

4.2 Grant the user called 'ansible' the permission to use sudo on the Ansible Server, Node1 and Node2.
```
visudo
```
add the following line under root privileges under the line ```root ALL=(ALL) ALL```
```
ansible ALL=(ALL) NOPASSWD=ALL
```
Save and quit

5.1 Create a group called 'demo' and declare the private IPs of Node1 and Node2 inside the inventory file of ansible. (/etc/ansible/hosts)
open the inventory file of ansible
```
vi /etc/ansible/hosts
```

Under the following statement you will find a space to declare groups and IPs: ```Ex 1: Ungrouped hosts, specify before any group headers.```
Write down the following under it:
```
[demo]
Private IP of Node1
Private IP of Node2
```
Save and Quit

5.2 In the Ansible configuration file uncomment the line which gives permission to read inventory file. Also, uncomment the line which gives sudo privileges to root user.
Open the configuration file
```
 vi /etc/ansible/ansible.cfg
```
Uncomment the following lines:
```
inventory      = /etc/ansible/hosts
```
```
sudo_user      = root
```

6.1 To SSH into the IPs of the Nodes from Ansible Server, some changes need to be made to the sshd_config file on the Ansible Server, Node1 and Node2.
Open the configuration file for SSH
```
vi /etc/ssh/sshd_config
```

Uncomment the following lines:
```
PermitRootLogin yes
```
```
PasswordAuthentication yes
```

Comment the following line:
```
PasswordAuthentication no
```
Save and Quit

6.2 Switch to Ansible user on all 3 EC2. You will now be able to SSH into the ansible user of Node1 and Node2 with the password that you had set for this user on the respective nodes.
```
su - ansible
```
```
ssh ansible@Private_IP
```

6.3 The problem with the above scenario is that it will ask for the password each and every time the ansible server wants to establish an SSH connection with the Nodes. To solve this problem we will generate an SSH key while switching to ansible user on all 3 EC2s and copying the ssh public key file to the nodes. We need to be ansible user on all the EC2s while doing these actions because sshkeygen can only extablish Host to Host or User to User relationships
```
ssh key-gen
```
If you `ls` into the hidden folder `.ssh`, you will find the public key in there. We need to copy this to both the nodes.
```
ssh-copy-id ansible@PrivateIPOfNode
```
Now try SSHing into Node1 and Node2 from the Ansible Server, it will not prompt you for the password.

7.1 Verify each managed node is able to be accessed by Ansible 
```
ansible -i /etc/ansible/hosts private-ip-of-node -m ping
```

7.2 Verify all nodes in the managed node group is able to be accessed by Ansible 
```
ansible -i /etc/ansible/hosts node-group-name -m ping
```

7.3 Redirect the output to a file call pingstatus.txt
```
ansible -i /etc/ansible/hosts node-group-name -m ping > pingstatus.txt
```

## Host Pattern
1. To view the IPs of all the hosts listed in the inventory file
```
ansible all --list-hosts
```

2. To view the all the IPs listed in a group
```
ansible group-name --lists-hosts
```

3. Display the IP of the first machine in the group
```
ansible group-name[0] --list-hosts
```

4. Display the IP of the second machine in the group
```
ansible group-name[1] --list-hosts
```

5. Display the IP of the last machine in the group
```
ansible group-name[-1] --list-hosts
```

6. Display the IP of the first and second machine in the group
```
ansible group-name[0:1] --list-hosts
```

7. To display the IP of multiple nodes in multiple groups
```
ansible group1[0:1]:group2[0:1]
```
OR
```
ansible group1:group2
```



