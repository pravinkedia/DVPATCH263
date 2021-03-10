# DVPATCH263

## Check the imageregistry version and location

###### oc project openshift-image-registry
###### oc describe configs.imageregistry.operator.openshift.io
###### oc edit configs.imageregistry.operator.openshift.io
###### oc describe rs image-registry-54579bd75d | grep CHUNK

## Install Podman on Bastion Node

###### mkdir podman
###### cd podman
###### wget https://download-ib01.fedoraproject.org/pub/epel/7/x86_64/Packages/f/fuse3-libs-3.6.1-2.el7.x86_64.rpm
###### wget https://download-ib01.fedoraproject.org/pub/epel/7/x86_64/Packages/f/fuse3-3.6.1-2.el7.x86_64.rpm
###### wget http://mirror.centos.org/centos/7/extras/x86_64/Packages/container-selinux-2.107-1.el7_6.noarch.rpm
###### sudo yum -y localinstall *rpm
###### sudo yum -y install podman

## Upload and Tag the images to the registry manually from the Bastion Node

###### sudo podman load /home/ec2-user/dv_patch_update/cpd-cli-workspace/images/dv-engine-----v1.5.0.0-263.tar default-route-openshift-image-registry.apps.ibmdv-sz-ocp.aipappsdv.com/cpd-meta-ops/dv-engine:v1.5.0.0-263

## Push the DV 263 Patch

#### Obtain the Credentials
###### oc whoami
###### oc whoami -t

#### Login to the podman
###### sudo podman login default-route-openshift-image-registry.apps.ibmdv-sz-ocp.aipappsdv.com/cpd-meta-ops --tls-verify=false

#### Push the image to registry
###### sudo podman push default-route-openshift-image-registry.apps.ibmdv-sz-ocp.aipappsdv.com/cpd-meta-ops/dv-engine:v1.5.0.0-263 --tls-verify=false

## Modify the yaml file to remove the problematic patch

###### cd cpd-cli-workspace/modules/dv/x86_64/1.5.0/patch/
###### cp v1.5.0.0-263.yaml org.v1.5.0.0-263.yaml
###### vi v1.5.0.0-263.yaml and remove the dv-engine:v1.5.0.0-263
###### vi v1.5.0.0-263.serviceinstance.yaml and remove the dv-engine:v1.5.0.0-263
###### cp v1.5.0.0-263.serviceinstance.yaml org.v1.5.0.0-263.serviceinstance.yaml

## Apply the Patch

###### ./cpd-cli patch -n cpd-sz --load-from /home/ec2-user/dv_patch_update/cpd-cli-workspace/ --assembly dv --patch-name v1.5.0.0-263 --transfer-image-to default-route-openshift-image-registry.apps.ibmdv-sz-ocp.aipappsdv.com/cpd-meta-ops --action push  --verbose --insecure-skip-tls-verify --target-registry-username $(oc whoami) --target-registry-password $(oc whoami -t)

###### ./cpd-cli patch -n cpd-sz --load-from /home/ec2-user/dv_patch_update/cpd-cli-workspace/ --assembly dv --patch-name v1.5.0.0-263 --transfer-image-to default-route-openshift-image-registry.apps.ibmdv-sz-ocp.aipappsdv.com/cpd-meta-ops --action push  --verbose --insecure-skip-tls-verify --target-registry-username $(oc whoami) --target-registry-password $(oc whoami -t) --all-instances

## Check if the DV engine service is up

###### watch "oc get po -n cpd-sz | grep dv"
