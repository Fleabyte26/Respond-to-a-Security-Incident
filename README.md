# Google Cloud Arcade - Respond to a Security Incident

## Solution (10 min lab)

### Task 1: Isolate compromised VM
```bash
gcloud compute firewall-rules delete critical-fw-rule --quiet && gcloud compute firewall-rules create critical-fw-rule --network=client-vpc --action=DENY --rules=tcp:80 --direction=INGRESS --source-ranges=0.0.0.0/0 --target-tags=compromised-vm --priority=1000
```

### Task 2: Allow SSH from bastion
```bash
gcloud compute firewall-rules delete allow-ssh-from-bastion --quiet 2>/dev/null; gcloud compute firewall-rules create allow-ssh-from-bastion --network=client-vpc --action allow --direction=ingress --rules tcp:22 --source-ranges=$(gcloud compute instances describe bastion-host --zone=$(gcloud compute instances list --filter="name=bastion-host" --format="get(zone)") --format="get(networkInterfaces[0].accessConfigs[0].natIP)") --target-tags=compromised-vm
```

### Task 3: Enable VPC flow logs
```bash
gcloud compute networks subnets update my-subnet --region=$(gcloud compute networks subnets list --filter="name=my-subnet" --format="get(region)") --enable-flow-logs

YouTube tutorial: 
