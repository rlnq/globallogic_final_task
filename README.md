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

Requirements:
az-cli

### Steps:

```
git clone https://github.com/rlnq/globallogic_final_task.git
cd globallogic_final_task
terraform init 
terraform plan
terraform apply
```

<img width="1440" alt="image" src="https://user-images.githubusercontent.com/117667360/219942435-7e2825a3-1302-4230-b470-0806257e3cbf.png">

* ### Clone Kubespray release repository:
```
git clone https://github.com/kubernetes-sigs/kubespray.git
cd kubespray
git checkout release-2.20
```
* ### Copy and edit inventory file
```
cp -rfp inventory/sample inventory/mycluster
nano inventory/mycluster/inventory.ini
```
<img width="1250" alt="image" src="https://user-images.githubusercontent.com/117667360/216760216-45ffe798-c5e4-47e7-81f1-0a222409027d.png">

* ### Turn on MetalLB

```
nano inventory/mycluster/group_vars/k8s_cluster/addons.yml
```
```
metallb_enabled: true
metallb_speaker_enabled: true
metallb_avoid_buggy_ips: true
metallb_ip_range:
  - "10.200.0.2/32"
# 10.200.0.2 VM private IP address
```
<img width="1207" alt="image" src="https://user-images.githubusercontent.com/117667360/217276964-a1119a50-e89b-4efd-ac7e-9184ca439e08.png">

```
nano inventory/mycluster/group_vars/k8s_cluster/k8s-cluster.yml
```
```
kube_proxy_strict_arp: true
```
<img width="1022" alt="image" src="https://user-images.githubusercontent.com/117667360/217277394-aa6d70fe-945f-43f8-8bc5-9f911459c61b.png">

* ### Run execute container
```
docker run --rm -it -v /*your_folder*/kubespray:/mnt -v ~/.ssh:/pem   quay.io/kubespray/kubespray:v2.20.0 bash
```

* ### Go to kubespray folder and start ansible-playbook
```
cd /mnt/kubespray
```

```
ansible-playbook -i inventory/mycluster/inventory.ini --private-key /pem/id_rsa -e ansible_user=root -b  cluster.yml
```
<img width="1440" alt="image" src="https://user-images.githubusercontent.com/117667360/217348736-a84dc206-6549-45da-a3b3-306cf2595f7e.png">

```
kubectl get nodes
kubectl get ns
```
<img width="1197" alt="image" src="https://user-images.githubusercontent.com/117667360/217352562-7ae90e21-6230-48fc-bee1-1597e515a5d9.png">
