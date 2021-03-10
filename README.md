# DVPATCH263

## Install Podman on Bastion Node

###### mkdir podman
###### cd podman
###### wget https://download-ib01.fedoraproject.org/pub/epel/7/x86_64/Packages/f/fuse3-libs-3.6.1-2.el7.x86_64.rpm
###### wget https://download-ib01.fedoraproject.org/pub/epel/7/x86_64/Packages/f/fuse3-3.6.1-2.el7.x86_64.rpm
###### wget http://mirror.centos.org/centos/7/extras/x86_64/Packages/container-selinux-2.107-1.el7_6.noarch.rpm
###### sudo yum -y localinstall *rpm
###### sudo yum -y install podman


## Upload the images to the registry manually from the Bastion Node


## Tag and Push the DV 263 Patch



## Modify the yaml file to remove the problematic patch



## Apply the Patch



