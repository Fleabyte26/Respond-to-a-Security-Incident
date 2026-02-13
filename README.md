# Google Cloud Arcade - Respond to a Security Incident

## Solution (10 min lab)

### Task 1: Isolate compromised VM
```bash
gcloud compute firewall-rules delete critical-fw-rule --quiet && gcloud compute firewall-rules create critical-fw-rule --network=client-vpc --action=DENY --rules=tcp:80 --direction=INGRESS --source-ranges=0.0.0.0/0 --target-tags=compromised-vm --priority=1000
```

### Task 2: Allow SSH from bastion
```bash
gcloud compute firewall-rules create allow-ssh-from-bastion --network=client-vpc --action=ALLOW --direction=INGRESS --rules=tcp:22 --source-tags=bastion --target-tags=compromised-vm --priority=900
```

### Task 3: Enable VPC flow logs
```bash
gcloud compute networks subnets update my-subnet --enable-flow-logs --region=us-east1
```

## Notes
- Deny **tcp:80** specifically (not all traffic)
- Bastion tag is `bastion` (not `bastion-host`)
- Paste full commands, don't split lines

---
YouTube tutorial: [your link]
