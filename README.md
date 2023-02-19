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
<img width="1437" alt="image" src="https://user-images.githubusercontent.com/117667360/219942979-616dac52-3bc6-4589-9d3a-d8c3511e6bb8.png">

* ### Turn on MetalLB

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

* ### Run execute container
```
docker run --rm -it -v /*your_folder*/kubespray:/mnt -v ~/.ssh:/pem   quay.io/kubespray/kubespray:v2.20.0 bash
```

* ### Go to kubespray folder and start ansible-playbook
```
cd /mnt/kubespray
```

```
ansible-playbook -i inventory/mycluster/inventory.ini --private-key /pem/id_rsa -e ansible_user=azureuser -b  cluster.yml
```
<img width="1440" alt="image" src="https://user-images.githubusercontent.com/117667360/217348736-a84dc206-6549-45da-a3b3-306cf2595f7e.png">

```
kubectl get nodes
kubectl get ns
```
<img width="1197" alt="image" src="https://user-images.githubusercontent.com/117667360/217352562-7ae90e21-6230-48fc-bee1-1597e515a5d9.png">
