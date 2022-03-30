## VSI Hardening and Users Management

This Repo contains files run the ansible playbook to manage the users and to perform server hardening tasks.

###Prerequisites 

The following tools need to be available on the machine where this playbook is being executed.

* ansible v2.7
* python 2.7 and above
* centos 7/8 server 

### Prepare the server

Create a server and assign a floating IP to it and ensure it is reachable from your network.

###### Folder Structure


```
.
├── README.md
├── cos_sat_users
├── inventory
├── keys
│   ├── mpcarl
│   │   └── id_rsa.pub
│   ├── msb
│   │   └── id_rsa.pub
│   ├── praveenbhat
│   │   └── id_rsa.pub
│   ├── shivaraj
│   │   └── id_rsa.pub
│   ├── simon
│   │   └── id_rsa.pub
│   └── vittalpai
│       └── id_rsa.pub
└── vsi-hardening.yml

7 directories, 10 files
```

Here are some definitions of the items used in the code above:

1. ```cos_sat_users``` : list of users. add your user entries here
2.  ```inventory```: it is inventory file for the play, insert the server ip and it port details in it.
3. ```keys```: is a directory where the added users .pub keys are stored, add the user .pub key by creating folder and file in it
4. ```vsi-hardening.yml```: it is the main playbook.


### Inputs to Playbook

 add your server IP and port details in inventory file

```
 cat inventory 
                                                                                                                                                                                                           
[root]
x.x.x.x ##### Insert the VSI IP ###########

[all:vars]
ansible_connection=ssh
ansible_user=root
ansible_port=22
ansible_pass=~/.ssh/id_rsa.pem
```
 add the list of users you want to add to server to the cos_sat_users file.

```
cat cos_sat_users 
                                                                                                                                                                                                      
cos_sat_users:
  - mpcarl
  - simon
  - vittalpai
  - praveenbhat
  - shivaraj
  - msb
```

add user id_rsa.pub key to '**/keys**/' folder. create a dedicated folder with the **username** and place ```id_rsa.pub``` key in the user file

Note: username in the keys directory and in the ```cos_sat_users``` should match. if not it the playbook gives an error.

```
keys
├── mpcarl
│   └── id_rsa.pub
├── msb
│   └── id_rsa.pub
├── praveenbhat
│   └── id_rsa.pub
├── shivaraj
│   └── id_rsa.pub
├── simon
│   └── id_rsa.pub
└── vittalpai
    └── id_rsa.pub

6 directories, 6 files
```
###Executing the Playbook

Once all the above mentioned steps are created, run below command from inside the vsi-hardening directory.


 ```ansible-playbook vsi-hardening.yml -i inventory```
 
 once the above commabd executed, the playbook will run the tasks on the server provided in inventory file and it should exit without any errors.
 
 ```
 ansible-playbook vsi-hardening.yml -i inventory                                                                                                                                                                  

Using /Users/simonraj/.ansible.cfg as config file

PLAY [all] *******************************************************************************************************************************************************************************************************************

TASK [Gathering Facts] *******************************************************************************************************************************************************************************************************
ok: [130.198.22.241]

TASK [add user to sudo group >>>> TASK1] *************************************************************************************************************************************************************************************
skipping: [130.198.22.241] => (item=mpcarl)  => {"ansible_loop_var": "item", "changed": true, "cmd": "useradd -m -d /home/\"mpcarl\" -s /bin/bash \"mpcarl\"\nsleep 1\nusermod -aG wheel \"mpcarl\"\n", "delta": null, "end": null, "item": "mpcarl", "msg": "Command would have run if not in check mode", "rc": 0, "start": null, "stderr": "", "stderr_lines": [], "stdout": "", "stdout_lines": []}
skipping: [130.198.22.241] => (item=simon)  => {"ansible_loop_var": "item", "changed": true, "cmd": "useradd -m -d /home/\"simon\" -s /bin/bash \"simon\"\nsleep 1\nusermod -aG wheel \"simon\"\n", "delta": null, "end": null, "item": "simon", "msg": "Command would have run if not in check mode", "rc": 0, "start": null, "stderr": "", "stderr_lines": [], "stdout": "", "stdout_lines": []}
skipping: [130.198.22.241] => (item=vittalpai)  => {"ansible_loop_var": "item", "changed": true, "cmd": "useradd -m -d /home/\"vittalpai\" -s /bin/bash \"vittalpai\"\nsleep 1\nusermod -aG wheel \"vittalpai\"\n", "delta": null, "end": null, "item": "vittalpai", "msg": "Command would have run if not in check mode", "rc": 0, "start": null, "stderr": "", "stderr_lines": [], "stdout": "", "stdout_lines": []}
.................
......................
............................

PLAY RECAP *************************************************************************************************************************************
node1                      : ok=16    changed=16    unreachable=0    failed=0   

```

 
