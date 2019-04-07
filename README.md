# ansible-k8s-simple-3node
Ansible playbook to bootstrap a simple k8s cluster consisting of 3 CentOs nodes. The playbook
- prepares all 3 nodes
- initalizes the cluster on the master node
- joins the cluster with the worker nodes
## Prerequisites
- 3 nodes with CentOS 7 minimal installation
- root access
- update `hosts` file with your ip addresses and hostnames as shown below
```
[master]
k8s-master ansible_host=10.0.0.100 ansible_user=root
[worker]
k8s-worker1 ansible_host=10.0.0.101 ansible_user=root
k8s-worker2 ansible_host=10.0.0.102 ansible_user=root
```
- modify `env_variables` file
```
# master ip address
api_addr: 10.0.0.100
# ssh key
ssh_key: "ssh-rsa YOUR PUBLIC KEY"
```
## Run playbook
Execute the playbook with the following command
```
sudo ansible-playbook -i hosts bootstrap-nodes.yml --ask-pass
``` 
## Verify installation
Execute the following commands on the master node to verify a successful installation
- `kubectl cluster-info`
```
Kubernetes master is running at https://10.0.0.100:6443
KubeDNS is running at https://10.0.0.100:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
```
- `kubectl get nodes --all-namespaces`
```
NAME          STATUS   ROLES    AGE   VERSION
k8s-master    Ready    master   16m   v1.14.0
k8s-worker1   Ready    <none>   15m   v1.14.0
k8s-worker2   Ready    <none>   15m   v1.14.0
```
