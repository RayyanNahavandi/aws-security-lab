AWS Secure Cloud Monitoring Lab
A hands-on cloud security project built on AWS Free Tier — covering VPC design, Linux server hardening, threat detection, and real-time network monitoring.
Project Summary
Built a fully functional secure cloud environment on AWS from scratch, implementing real-world networking, server hardening, and cybersecurity monitoring practices used in enterprise environments.
Detected a real GuardDuty security finding (root credential misuse, 17 occurrences) and remediated it with proper IAM configuration.

Architecture
Internet
    |
    v
Internet Gateway (Secure-igw)
    |
    v
VPC: 10.0.0.0/16 (Secure-vpc) -- us-east-2
    |-- Public Subnet (10.0.0.0/20) -- us-east-2a
    |       |-- EC2: Security-Lab-Server (t2.micro)
    |               |-- Nginx Web Server
    |               |-- Fail2ban IPS
    |-- Public Subnet -- us-east-2b
    |-- Private Subnet -- us-east-2a (reserved)
    |-- Private Subnet -- us-east-2b (reserved)

Technologies Used
ServicePurposeAmazon VPCIsolated network with public/private subnetsAmazon EC2 (t2.micro)Amazon Linux 2023 web serverAWS GuardDutyThreat detection and anomaly monitoringVPC Flow LogsFull network traffic capture to CloudWatchAWS CloudTrailAPI call auditingAWS IAMLeast-privilege access control with MFANginxWeb serverFail2banBrute-force SSH prevention

What Was Built
1. Network Infrastructure

Custom VPC (10.0.0.0/16) with 4 subnets across 2 Availability Zones
Internet Gateway with public route table
Private subnets isolated for future backend use

2. EC2 Server Hardening
bash# Update system
sudo dnf update -y

# Install and enable Fail2ban (brute-force protection)
sudo dnf install -y fail2ban
sudo systemctl enable --now fail2ban

# Disable root SSH login
sudo sed -i 's/#PermitRootLogin yes/PermitRootLogin no/' /etc/ssh/sshd_config
sudo systemctl restart sshd

# Deploy Nginx web server
sudo dnf install -y nginx
sudo systemctl enable --now nginx
3. Security Monitoring

VPC Flow Logs sending to CloudWatch (/var/security-lab), capturing all ACCEPT/REJECT traffic
AWS GuardDuty enabled for continuous threat intelligence
Billing alerts configured to prevent unexpected charges

4. IAM Hardening

Created dedicated IAM user with least-privilege access instead of using root
Enabled MFA on both IAM user and root account
Remediated GuardDuty finding: Policy:IAMUser/RootCredentialUsage


Security Finding Detected and Remediated
Finding: Policy:IAMUser/RootCredentialUsage
Severity: Low | Count: 17
Description: GuardDuty detected root account credentials being used directly for API calls, violating least-privilege security principles.
Remediation:

Created rayyan-admin IAM user with AdministratorAccess policy
Enabled MFA on root account
Stopped using root credentials for all operations


Flow Log Analysis
Real traffic captured from VPC Flow Logs in CloudWatch:
ACCEPT  -- Port 80  (HTTP traffic to Nginx)
ACCEPT  -- Port 22  (SSH from authorized IP)
REJECT  -- Various  (Unauthorized probe attempts blocked by Security Group)

Skills Demonstrated

Networking: VPC design, CIDR subnetting, routing, firewall rules
Systems: Linux administration, SSH hardening, service management
Cybersecurity: IDS/IPS, log analysis, threat detection, IAM security
Cloud (AWS): EC2, VPC, GuardDuty, CloudWatch, IAM, CloudTrail
Scripting: Bash hardening scripts, HTML/CSS web deployment


Cost
Built entirely on AWS Free Tier -- approximately $0 (12-month free services + GuardDuty 30-day trial).

Built March 2026 | Rayyan Nahavandi
