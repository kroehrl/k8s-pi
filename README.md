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
```
ssh root@<ip-address>
```

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
```
ssh <username>@<ip-address>
```










### TODO:
- [ ] Describe the manjaro minimal setup
- [ ] Describe the ansible setup (f. e. by using pip and the requirements.txt)
- [ ] Edit the ansible.cfg especially for the remote_user