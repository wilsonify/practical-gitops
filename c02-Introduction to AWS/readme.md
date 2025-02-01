# AWS EC2 Instance Setup Guide

## Introduction

This guide provides step-by-step instructions for launching, accessing, and terminating an AWS EC2 instance using both
the AWS Management Console (GUI) and AWS CLI.

---

## 1. Launching an EC2 Instance (AWS Management Console)

### Steps:

1. **Select an AMI (Amazon Machine Image):** Choose Ubuntu.
2. **Choose an Instance Type:** Select `t2.micro` for free-tier eligibility.
3. **Configure Key Pair:**
    - Click **Download Key Pair** and store it securely.
4. **Launch Instance:**
    - Click **Launch Instances**.
5. **View Instance Details:**
    - Navigate to EC2 dashboard and locate the **Public IP Address**.
6. **Access the Instance:**
    - Copy the Public IP and paste it into your browser.

### Terminating the Instance:

1. Click **Instance State** → **Terminate Instance**.
2. Wait approximately 5 minutes for termination.

---

## 2. Launching an EC2 Instance (AWS CLI)

### Prerequisites:

- AWS CLI installed and configured with valid credentials.
- Set AWS profile:
  ```sh
  export AWS_PROFILE=gitops
  ```

### Required Parameters:

- **AMI ID:** `ami-00399ec92321828f5`
- **Instance Type:** `t2.micro`
- **Key Name:** `gitops`
- **Security Group ID:** (Retrieve using CLI)
- **Subnet ID:** (Retrieve using CLI)
- **User Data File:** `user_data.sh`

### Fetching Security Group ID:

```sh
aws ec2 describe-security-groups --query 'SecurityGroups[*].[GroupName,GroupId]'
```

### Fetching Subnet ID:

```sh
aws ec2 describe-subnets --query 'Subnets[*].[SubnetId,AvailabilityZone]'
```

### Running the EC2 Instance:

```sh
aws ec2 run-instances --image-id ami-00399ec92321828f5 \
  --count 1 --instance-type t2.micro --key-name gitops \
  --security-group-ids sg-085db3a64b72894be \
  --subnet-id subnet-79b62812 --user-data file://user_data.sh
```

### Retrieving Public IP Address:

```sh
aws ec2 describe-instances \
  --filters "Name=instance-state-name,Values=running" "Name=instance-id,Values=<INSTANCE_ID>" \
  --query 'Reservations[*].Instances[*].[PublicIpAddress]' --output text
```

### Accessing the Instance:

```sh
curl -I http://<PUBLIC_IP>
```

### Terminating the Instance:

```sh
aws ec2 terminate-instances --instance-id <INSTANCE_ID>
```

---

## 3. Clean-Up

- Verify that all instances are terminated by checking the **EC2 Console**.
- Ensure no unintended resources are left running.

---

## 4. Conclusion

- AWS EC2 allows quick deployment of virtual machines using GUI or CLI.
- GUI is user-friendly but lacks automation capabilities.
- CLI provides automation but requires manual tracking.
- The next step involves using **Terraform** to manage infrastructure efficiently.

---

### Author:

This guide is created for individuals looking to quickly set up an EC2 instance on AWS using GUI or CLI.

### License:

MIT License

