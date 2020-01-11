# k8s-pi
Kubernetes Cluster on Manjaro (ARM64)

## Table of contents:
- [Manjaro](#manjaro)
- [TODO](#todo)


## Install Manjaro
First of all you have to download the Manjaro ARM Image. In this case I used the Minimal Manjaro Arm Image for the Raspberry Pi 4 (I have tested this guide with version 19.12 of Manjaro ARM):

[Download Manjaro ARM for Raspberry Pi 4](https://manjaro.org/download/#raspberry-pi-4-minimal)


You can use [Etcher](https://www.balena.io/etcher/) to flash the image to the SD Card. After you are done, place the SD-Cards in the RaspberryPis.


Access the Raspberry Pis by using ssh:
```bash
ssh root@<ip-address>
```
---
**NOTE**
You have to replace the ```<ip-address>``` placeholder.

---

A setup dialog will start where you have to configure the following items:
- keyboard layout
- user
    - username
    - password
    - additional user groups _(can be skipped)_
    - full name
- root password
- timezone
- locale
- hostname

After you are done configuring your Manjara ARM your Raspberry Pi will reboot.
After that you can login by using your **username** instead of **root**:
```bash
ssh <username>@<ip-address>
```
---
**NOTE**
You have to replace the ```<username>``` and ```<ip-address>``` placeholders.

---

## Setting up public key authentication

Based on [this](https://www.ssh.com/ssh/copy-id/) description we will setup a public key authentification. This makes it easier to access the RaspberryPis by using Ansible without the need of a password to login.

### Create an public and private key file:
```bash
~ ❯ ssh-keygen              
Generating public/private rsa key pair.
Enter file in which to save the key (/home/white/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/white/.ssh/id_rsa.
Your public key has been saved in /home/white/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:73mJv0tQer6JDlPq6Xw4kl4RBkF/tlaZ1ts6WmxNk+U white@white-Laptop
The key's randomart image is:
+---[RSA 3072]----+
|     .+.         |
|       o     +   |
|        + o * . .|
|       . + *   +o|
|        S * . .oE|
|         * + . +.|
|       .=...o.* .|
|      o+o=o+o* . |
|     ..o=+=oBo   |
+----[SHA256]-----+
```

### Copy the SSH key to the RaspberryPi
```bash
ssh-copy-id -i ${HOME}/.ssh/id_rsa <username>@<ip-address>
```
---
**NOTE**
You have to replace the ```<username>``` and ```<ip-address>``` placeholders.

---

Repeat this step for all your RaspberryPis. You should now be able to login to your RaspberryPis by ssh without a password.


## Setup k8s

### Install Ansible
You can install ansible by following the [official ansible installation guide](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-the-control-node)


### Create the configuration files
- **Rename or copy the `ansible.cfg.sample` as `ansible.cfg` in the project root.** <br>
Adapt the file to your needs. The user for whom you have set up the passwordless SSH login should be entered as ```remote_user```.

- **Rename or copy the `hosts.ini.sample` as `hosts.ini` in the inventory folder.** <br>
Adapt the file to your needs. Escpecially you have to change the hostnames and the IP-Addresses. Furthermore if you use more/less nodes you have to add/remove hosts.

- **Rename or copy the `secrets.yml.template` as `secrets.yml` in the secrets folder.** <br>
Adapt the file to your needs. This file contains also the versions of your deployments which will be used to upgrade your cluster.

### Bootstrap the cluster
If everything is configured you should be able to bootstrap the cluster by executing the following command:
```bash
ansible-playbook -i inventory/hosts.ini --extra-vars @secrets/secrets.yml bootstrap.yml -K
```

### Upgrade the cluster
If you want to upgrade your cluster you just have to execute the following command. This will also trigger the `deploy.yml` playbook.
```bash
ansible-playbook -i inventory/hosts.ini --extra-vars @secrets/secrets.yml upgrade.yml -K
```