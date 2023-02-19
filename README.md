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
