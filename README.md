# Final Task

### Task:

#### Terraform:
* create VPC in GCP/Azure						
* create instance with External IP						
* prepare managed DB (MySQL)						
							
#### Ansible:	
* perform basic hardening (keyless-only ssh, unattended upgrade, firewall)						
* (optional) perform hardening to reach CIS-CAT score at least 80 (please find https://learn.cisecurity.org/cis-cat-lite)				
* deploy K8s (single-node cluster via Kubespray)						
							
#### Kubernetes:	
* prepare ansible-playbook for deploying Wordpress						
* deploy WordPress with connection to DataBase	

--------------------------------

### Requirements:
* azure free trial account
* azure-cli v2.45.0
* terraform v1.3.7
* docker v20.10.22

-------------------------------

## Part 1 - Terraform
* create VPC in GCP/Azure						
* create instance with External IP						
* prepare managed DB (MySQL)	

### Steps: 

* Clone terraform code to our local machines 
```
git clone https://github.com/rlnq/globallogic_final_task.git
```
* Go to project folder
```
cd globallogic_final_task
```
* Initialization
```
terraform init 
```
* This will show you a preview of the infrastructure that will be created based on your configuration file.
```
terraform plan
```
* Type terraform apply, and press Enter. This will create the specified infrastructure.
```
terraform apply
```

<img width="1440" alt="image" src="https://user-images.githubusercontent.com/117667360/219942435-7e2825a3-1302-4230-b470-0806257e3cbf.png">

-----------------

## Part 2 - Ansible
* perform basic hardening (keyless-only ssh, unattended upgrade, firewall)						
* (optional) perform hardening to reach CIS-CAT score at least 80 (please find https://learn.cisecurity.org/cis-cat-lite)				
* deploy K8s (single-node cluster via Kubespray)


## Steps:
### perform basic hardening (keyless-only ssh, unattended upgrade, firewall)
* in process
### (optional) perform hardening to reach CIS-CAT score at least 80 (please find https://learn.cisecurity.org/cis-cat-lite)
* in process
### deploy K8s (single-node cluster via Kubespray)

* Clone Kubespray release repository to local machine:
```
git clone https://github.com/kubernetes-sigs/kubespray.git
cd kubespray
git checkout release-2.20
```
* Copy and edit inventory file
```
cp -rfp inventory/sample inventory/mycluster
nano inventory/mycluster/inventory.ini
```
<img width="1437" alt="image" src="https://user-images.githubusercontent.com/117667360/219942979-616dac52-3bc6-4589-9d3a-d8c3511e6bb8.png">

* Turn on MetalLB
```
nano inventory/mycluster/group_vars/k8s_cluster/addons.yml
```
```
# MetalLB deployment
metallb_enabled: true
metallb_speaker_enabled: true
metallb_ip_range:
  - "10.0.1.4"
metallb_avoid_buggy_ips: true
```
<img width="700" alt="image" src="https://user-images.githubusercontent.com/117667360/219943052-f23b3406-6c15-414b-ad61-e2bbdd66519e.png">

```
nano inventory/mycluster/group_vars/k8s_cluster/k8s-cluster.yml
```
```
kube_proxy_strict_arp: true
```
* Run execute container
```
docker run --rm -it -v /*your_folder*/kubespray:/mnt -v ~/.ssh:/pem   quay.io/kubespray/kubespray:v2.20.0 bash
```
* Go to kubespray folder
```
cd /mnt
```
* Start ansible-playbook with inventory file and  private key
```
ansible-playbook -i inventory/mycluster/inventory.ini --private-key /pem/id_rsa -e ansible_user=azureuser -b  cluster.yml
```
<img width="1440" alt="image" src="https://user-images.githubusercontent.com/117667360/217348736-a84dc206-6549-45da-a3b3-306cf2595f7e.png">

```
kubectl get nodes
kubectl get ns
```
