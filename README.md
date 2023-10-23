# Ansible K3s

Create a bare metal Kubernetes cluster using Ansible, Ubuntu, KVM and k3s.
Tested on Ubuntu 22.04.

The configuration has a few hardcoded values. It has not been abstracted for general consumption.

This Ansible playbook will:
- Connect to an Ubuntu server.
- Setup the KVM (libvirt) hypervisor.
- Create VMs for each of the k3s nodes.
- Setup a k3s cluster and connect all nodes.
- Setup HAProxy on the host, with TLS passthrough to the cluster.
- Installs `kubectl` on the localhost, with a valid `kubeconfig` file (saving
    any existing configuration).


## Tech Stack

- Ansible configuration management
- Ubuntu host machine
- HAProxy
- LibVirt (KVM) hypervisor
- Ubuntu cloud VM images
- Cloud-Init configuration files
- K3s


## Installation

Install Python dependencies.

```
python3 -m venv ./.venv
source .venv/bin/activate
pip install -r requirements.txt
```

Install Ansible dependencies.

```
ansible-galaxy install -r requirements.yaml
```


## Usage

### Configuration

Have an Ubuntu 22.04 server setup, and make sure you can SSH into it.

Customize the `inventory` file. Specifically:
- Update the public key with the one on your machine, used to connect
  to the Ubuntu KVM server.
- Update the memory allocation size for each VM.
- Update the source and location where ArgoCD pulls the Git SSH key.
    Make sure the SSH public key is set as a
    [deploy key](https://docs.github.com/en/developers/overview/managing-deploy-keys#deploy-keys)
    for the remote repo, and the private key is set in the specified location.

### Execution

Run the playbook.

```
ansible-playbook -K playbook.yaml
```
