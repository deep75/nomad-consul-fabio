# nomad-in-a-box

# how to use

## deploy in a fresh Ubuntu 18.04 VM
```
sudo ./scripts/provision.sh 
```

Vault root token is `changeme`

Then you can reach the services at

- consul http://<IP of VM>:8500
- nomad http://<IP of VM>:4646
- vault http://<IP of VM>:8200


### vault

Vault is running on dev mode in dc1, so you can access the service as:

- `vault.service.consul` in the dc1
- `vault.service.dc1.consul` in the dc2

### terraform and nomad

in `tf_local` there is a terraform project you can use to schedule a sample job in the nomad cluster with Terraform.

When this job runs:
- a job named hello will run in nomad, count 4, with a basic webpage
- a service named `hello` will be registered in consul
- consul-template will configure nginx on port 8080 on the VM

```
Hello! We are running on
datacenter: dcN
hostname: clientN-dcN
port: 8080
```

## description

This projects uses LXD to create containers.

```
# lxc list
+-------------+---------+--------------------------------+------+------------+-----------+
|    NAME     |  STATE  |              IPV4              | IPV6 |    TYPE    | SNAPSHOTS |
+-------------+---------+--------------------------------+------+------------+-----------+
| base        | STOPPED |                                |      | PERSISTENT | 0         |
+-------------+---------+--------------------------------+------+------------+-----------+
| base-client | STOPPED |                                |      | PERSISTENT | 0         |
+-------------+---------+--------------------------------+------+------------+-----------+
| client1-dc1 | RUNNING | 172.17.0.1 (docker0)           |      | PERSISTENT | 0         |
|             |         | 10.170.13.41 (eth0)            |      |            |           |
+-------------+---------+--------------------------------+------+------------+-----------+
| client1-dc2 | RUNNING | 172.17.0.1 (docker0)           |      | PERSISTENT | 0         |
|             |         | 10.170.14.41 (eth0)            |      |            |           |
+-------------+---------+--------------------------------+------+------------+-----------+
| client2-dc1 | RUNNING | 172.17.0.1 (docker0)           |      | PERSISTENT | 0         |
|             |         | 10.170.13.42 (eth0)            |      |            |           |
+-------------+---------+--------------------------------+------+------------+-----------+
| client2-dc2 | RUNNING | 172.17.0.1 (docker0)           |      | PERSISTENT | 0         |
|             |         | 10.170.14.42 (eth0)            |      |            |           |
+-------------+---------+--------------------------------+------+------------+-----------+
| consul1-dc1 | RUNNING | 10.170.13.11 (eth0)            |      | PERSISTENT | 0         |
+-------------+---------+--------------------------------+------+------------+-----------+
| consul1-dc2 | RUNNING | 10.170.14.11 (eth0)            |      | PERSISTENT | 0         |
+-------------+---------+--------------------------------+------+------------+-----------+
| consul2-dc1 | RUNNING | 10.170.13.12 (eth0)            |      | PERSISTENT | 0         |
+-------------+---------+--------------------------------+------+------------+-----------+
| consul2-dc2 | RUNNING | 10.170.14.12 (eth0)            |      | PERSISTENT | 0         |
+-------------+---------+--------------------------------+------+------------+-----------+
| consul3-dc1 | RUNNING | 10.170.13.13 (eth0)            |      | PERSISTENT | 0         |
+-------------+---------+--------------------------------+------+------------+-----------+
| consul3-dc2 | RUNNING | 10.170.14.13 (eth0)            |      | PERSISTENT | 0         |
+-------------+---------+--------------------------------+------+------------+-----------+
| nomad1-dc1  | RUNNING | 10.170.13.31 (eth0)            |      | PERSISTENT | 0         |
+-------------+---------+--------------------------------+------+------------+-----------+
| nomad1-dc2  | RUNNING | 10.170.14.31 (eth0)            |      | PERSISTENT | 0         |
+-------------+---------+--------------------------------+------+------------+-----------+
| nomad2-dc1  | RUNNING | 10.170.13.32 (eth0)            |      | PERSISTENT | 0         |
+-------------+---------+--------------------------------+------+------------+-----------+
| nomad2-dc2  | RUNNING | 10.170.14.32 (eth0)            |      | PERSISTENT | 0         |
+-------------+---------+--------------------------------+------+------------+-----------+
| nomad3-dc1  | RUNNING | 10.170.13.33 (eth0)            |      | PERSISTENT | 0         |
+-------------+---------+--------------------------------+------+------------+-----------+
| nomad3-dc2  | RUNNING | 10.170.14.33 (eth0)            |      | PERSISTENT | 0         |
+-------------+---------+--------------------------------+------+------------+-----------+
| vault1-dc1  | RUNNING | 10.170.13.21 (eth0)            |      | PERSISTENT | 0         |
+-------------+---------+--------------------------------+------+------------+-----------+
# 

```

# Screenshots
- consul
![screenshots/consul_dc.png](screenshots/consul_dc.png)
![screenshots/consul_hello_dc1.png](screenshots/consul_hello_dc1.png)
![screenshots/consul_hello_dc2.png](screenshots/consul_hello_dc2.png)
![screenshots/consul_kv_vault.png](screenshots/consul_kv_vault.png)
![screenshots/consul_nodes_dc1.png](screenshots/consul_nodes_dc1.png)
![screenshots/consul_nodes_dc2.png](screenshots/consul_nodes_dc2.png)
![screenshots/consul_nodes_vault.png](screenshots/consul_nodes_vault.png)
![screenshots/consul_services_dc1.png](screenshots/consul_services_dc1.png)
![screenshots/consul_services_dc2.png](screenshots/consul_services_dc2.png)

- nomad
![screenshots/nomad_client_driver.png](screenshots/nomad_client_driver.png)
![screenshots/nomad_clients.png](screenshots/nomad_clients.png)
![screenshots/nomad_job_hello.png](screenshots/nomad_job_hello.png)
![screenshots/nomad_no_job.png](screenshots/nomad_no_job.png)
![screenshots/nomad_servers.png](screenshots/nomad_servers.png)

- vault
![screenshots/vault_login.png](screenshots/vault_login.png)
![screenshots/vault_secret_engine.png](screenshots/vault_secret_engine.png)
