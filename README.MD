# Installation

On your working machine:
1. [Install kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
2. Install Ansible
```
$ python3 -m venv venv
$ . venv/bin/activate
(venv) $ pip install -r requirements
```

# Nodes configuration

Ensure that your nodes are accessible from your working machine via ssh.

> Important: nodes should have the same sudo password for ansible `become` to work properly!

Create `hosts.ini` with `master` node and `[worker_nodes]` section.
Example configs:

```
[master]
192.168.1.1 # replace with your

[worker_nodes]
node1 # entry from ~/.ssh/config
192.168.1.2 # plain ip
node3.mydomain.com # domain name
```
`example.hosts.ini` contains example of 2-node cluster with 1 worker node (`master` is registered at `~/.ssh/config`):
```
master

[worker_nodes]
node1
```

# Running Ansible Playbooks

```
(venv) $ ansible-playbook ansible/main.yml -K -i hosts.ini
```

# Accessing cluster after installation

Use `--kubeconfig=ansible/master_files/admin.conf` with `kubectl`, for example:

```
$ kubectl --kubeconfig=ansible/master_files/admin.conf get pods --all-namespaces
```

Repo contains files for [dashboard](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/) deployment:

```
$ kubectl --kubeconfig=ansible/master_files/admin.conf apply -R -f kubernetes/
```