Deployment of a mongodb replicaset
-----------------------------------------------------

This playbook setup a MongoDB replicaset with
* three nodes: primary, secondary and arbiter
* user authentication / authorization
* secure communication between the nodes (using key file)
* possibility to add more socondaries

Inventory
---------

The inventory file (inventory.ini) defines the hosts (primary and secondaries) used for the replicaset.  
Also, there are some database parameters (users and passwords) are defined:

```
[primary]
PRIMARY_IP

[secondary]
SECONDARY_1_IP

[arbiter]
ARBITER_IP

[primary:vars]
db_user_admin_username=USER_ADMIN_USERNAME
db_user_admin_password=USER_ADMIN_PASSWORD
db_cluster_admin_username=CLUSTER_ADMIN_USERNAME
db_cluster_admin_password=CLUSTER_ADMIN_PASSWORD
db_user_name=DB_USERNAME
db_user_password=DB_PASSWORD
db_name=DB_NAME
```

Usage
-----

- Clone git repository

```
git@github.com:esharanins/MongoDB-replicaset.git
```

- Correct inventory.ini as you need

- Run playbook to create MongoDB cluster  

With root authentication, run:
```
ansible-playbook -i inventory.ini main.yml -u root -k
```

**Note**: root ssh password will be requested  

  With key authentication and SUDO, run:
```
ansible-playbook -i inventory.ini main.yml -K
```  
**Note**: to be able to enter key password for multiple hosts, run:
``` eval "$(ssh-agent -s)"; ssh-add PATH_TO_PRIVATE_KEY ```  

- If you need to add secondary nodes, just add IP addresses or DNS names to еру `inventory.ini` in the `[secondary]` section

**Note**: if you need to remove some socondary nodes, you have to do it manually. On the primary node run:

```
mongo -u <usr> -p <pwd> --eval 'rs.remove("<secondary_ip>:<port>")'
```
... and voila!
