# AWS VPC Setup Guide (Complete Lab)

## Objective

This lab demonstrates how to build a fully functional AWS networking architecture using a custom VPC, public/private subnets, internet gateway, NAT gateway, route tables, and EC2 instances with secure SSH access.

By the end of this lab, you will have:

- A custom VPC (`10.0.0.0/16`)
- One public subnet and one private subnet
- Internet Gateway attached to VPC
- NAT Gateway for private subnet outbound internet access
- Separate route tables for public and private traffic
- Two EC2 instances (public + private)
- SSH access using bastion (public instance)

---

## Prerequisites

| Requirement | Description |
|------------|-------------|
| AWS Account | Active AWS account |
| IAM Permissions | EC2 + VPC full access |
| Key Pair | `.pem` file downloaded |
| Region | Same AWS region for all resources |
| SSH Client | Terminal / PowerShell |
| Basic AWS Knowledge | EC2 and VPC fundamentals |

---

## Architecture Summary

| Component | Purpose |
|----------|--------|
| VPC | 10.0.0.0/16 isolated network |
| Public Subnet | Hosts bastion EC2 |
| Private Subnet | Hosts backend EC2 |
| Internet Gateway | Public internet access |
| NAT Gateway | Private subnet outbound internet |
| Route Tables | Traffic routing control |

---

# LAB STEPS

---

## Step 1: Open AWS VPC Console
- Go to AWS Management Console
- Search and open **VPC service**

---

## Step 2: Create VPC

1. Click **Your VPCs**
2. Click **Create VPC**

### Configuration:

| Setting | Value |
|--------|------|
| Type | VPC only |
| Name | myvpc001 |
| IPv4 CIDR | 10.0.0.0/16 |
| IPv6 | No IPv6 |

3. Click **Create VPC**

---

## Step 3: Verify VPC
Ensure:
- VPC Name: `myvpc001`
- CIDR: `10.0.0.0/16`
- State: Available

---

# SUBNET CONFIGURATION

---

## Step 4: Create Subnets

Go to:
- VPC Dashboard → Subnets → Create subnet

---

## Step 5: Create Private Subnet

| Setting | Value |
|--------|------|
| VPC | myvpc001 |
| Name | mypvtsubnet001 |
| CIDR | 10.0.1.0/24 |

- Select Availability Zone
- Click **Create subnet**

---

## Step 6: Create Public Subnet

| Setting | Value |
|--------|------|
| VPC | myvpc001 |
| Name | mypubsubnet001 |
| CIDR | 10.0.2.0/24 |

- Select same/different AZ
- Click **Create subnet**

---

## Step 7: Verify Subnets
You should see:

- mypubsubnet001 (Public)
- mypvtsubnet001 (Private)

---

## Step 8: Enable Public IP Auto-Assignment

1. Select **mypubsubnet001**
2. Click **Actions → Edit subnet settings**
3. Enable:
   - Auto-assign public IPv4 address
4. Save

---

# INTERNET GATEWAY

---

## Step 9: Create Internet Gateway

1. Go to **Internet Gateways**
2. Click **Create internet gateway**

| Setting | Value |
|--------|------|
| Name | myigw001 |

---

## Step 10: Attach IGW to VPC

1. Select Internet Gateway
2. Click **Actions → Attach to VPC**
3. Select `myvpc001`

---

## Step 11: Verify IGW
Status must be:
- Attached

---

# NAT GATEWAY

---

## Step 12: Allocate Elastic IP

- Go to **Elastic IPs**
- Click **Allocate Elastic IP**

---

## Step 13: Create NAT Gateway

Go to **NAT Gateways → Create**

| Setting | Value |
|--------|------|
| Name | mynat001 |
| Subnet | Public subnet |
| Connectivity | Public |
| Elastic IP | Select allocated IP |

Click **Create NAT Gateway**

---

## Step 14: Wait for NAT Gateway
Status must become:
- Available

---

# ROUTE TABLE CONFIGURATION

---

## Step 15: Create Public Route Table

| Setting | Value |
|--------|------|
| Name | myroutepub001 |
| VPC | myvpc001 |

---

## Step 16: Add Public Route

Edit routes:

| Destination | Target |
|------------|--------|
| 0.0.0.0/0 | Internet Gateway |

Save changes.

---

## Step 17: Associate Public Subnet

1. Open **Subnet Associations**
2. Click **Edit**
3. Select:
   - mypubsubnet001
4. Save

---

## Step 18: Create Private Route Table

| Setting | Value |
|--------|------|
| Name | myroutepvt001 |
| VPC | myvpc001 |

---

## Step 19: Add Private Route

| Destination | Target |
|------------|--------|
| 0.0.0.0/0 | NAT Gateway |

Save changes.

---

## Step 20: Associate Private Subnet

1. Open subnet associations
2. Select:
   - mypvtsubnet001
3. Save

---

# EC2 INSTANCE SETUP

---

## Step 21: Launch EC2 Instances

Create two instances:

| Instance | Subnet | Role |
|----------|--------|------|
| ec2pub | Public subnet | Bastion host |
| ec2pvt | Private subnet | Backend server |

---

## Step 22: Configure Security Group

Allow:

| Type | Port | Source |
|------|------|--------|
| SSH | 22 | Your IP / Bastion SG |

---

## Step 23: Connect to Public EC2

### Set permission
```bash
chmod 400 key.pem
```

### SSH into public instance
```bash
ssh -i key.pem ec2-user@<public-ip-of-ec2pub>
```

---

## Step 24: Copy PEM file to Bastion (optional)

From local machine:

```bash
scp -i key.pem key.pem ec2-user@<public-ip-of-ec2pub>:/home/ec2-user/
```

---

## Step 25: Set permissions on Bastion

Inside ec2pub:

```bash
chmod 400 key.pem
```

---

## Step 26: SSH into Private EC2

From ec2pub:

```bash
ssh -i key.pem ec2-user@<private-ip-of-ec2pvt>
```

---

# EXPECTED OUTCOME

After completion:

- VPC fully operational
- Public subnet accessible from internet
- Private subnet isolated but internet-enabled via NAT
- EC2 public instance reachable via SSH
- EC2 private instance accessible only via bastion

---

# BEST PRACTICES

## Security
- Never expose private EC2 directly
- Use security groups instead of open access
- Keep `.pem` file secure

## Networking
- Always separate public/private subnets
- Use NAT Gateway (not NAT instance)
- Avoid 0.0.0.0/0 on private EC2 security groups

## Operations
- Use proper tagging
- Maintain naming conventions
- Monitor VPC Flow Logs

---

# TROUBLESHOOTING

| Issue | Cause | Fix |
|------|------|-----|
| Cannot SSH | SG blocked | Allow port 22 |
| No internet in public subnet | No IGW route | Add IGW route |
| Private EC2 unreachable | Wrong routing | Check NAT route |
| SSH fails | Wrong permissions | chmod 400 key.pem |

---

# FINAL RESULT

A secure AWS architecture with:
- Segmented network design
- Controlled internet access
- Bastion-based SSH access
- Production-style VPC setup
