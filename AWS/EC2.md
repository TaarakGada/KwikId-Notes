# EC2 — Elastic Compute Cloud

## What is it?
- EC2 is basically a **virtual computer** you rent from AWS in the cloud
- You don't need to buy physical servers — just spin up an EC2 instance, use it, and shut it down
- Think of it like renting an apartment instead of buying a house

## Key Concepts
- **Instance**: The virtual machine itself
- **AMI (Amazon Machine Image)**: The template/blueprint used to create an instance (like an OS image)
- **Instance Type**: Determines CPU, RAM, and storage (e.g., `t2.micro`, `c5.large`)
- **Key Pair**: SSH key to securely log into your instance
- **Security Group**: Firewall rules — what traffic is allowed in/out
- **Elastic IP**: A fixed public IP address you can attach to an instance

## Common Instance Types
| Type | Use Case |
|------|----------|
| `t3.micro` | Small apps, free tier |
| `c5.large` | CPU-heavy tasks |
| `r5.large` | Memory-heavy tasks |
| `p3.xlarge` | GPU / ML workloads |

## Connecting to EC2 via SSH
```bash
# Change permissions on your .pem key file first
chmod 400 my-key.pem

# Connect
ssh -i my-key.pem ec2-user@<your-public-ip>
```

## Lifecycle of an Instance
- **Start** → Running (billing starts)
- **Stop** → Stopped (no CPU billing, storage still billed)
- **Terminate** → Deleted permanently

## Good to Know
- EC2 instances live inside a **VPC** (Virtual Private Cloud) — your own isolated network
- You can attach **EBS volumes** (like external hard drives) to store data
- **Auto Scaling** lets you automatically add/remove instances based on load
- **Load Balancer** sits in front of multiple EC2 instances to distribute traffic
