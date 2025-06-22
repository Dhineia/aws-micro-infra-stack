📌 Project Walkthrough: Step-by-Step Deployment
✅ Step 1: Launch and Configure EC2 Instances
- Created a custom VPC with public subnets across two availability zones.
- Launched two Amazon Linux 2023 EC2 instances.
- Configured security groups to allow HTTP (port 80) and SSH (port 22).
- Installed Apache on both instances and served a simple HTML page:

In Bash:
sudo dnf install httpd -y
sudo systemctl start httpd
echo "Hello from $(hostname -f)" | sudo tee /var/www/html/index.html

✅ Step 2: Create and Configure the Application Load Balancer (ALB)
- Provisioned an internet-facing ALB spanning both public subnets.
- Created a Target Group with both EC2 instances registered.
- Set health check path to / to monitor web server availability.
- Verified that both instances were marked healthy

✅ Step 3: Test Load Balancer Routing
- Retrieved the DNS name of the ALB and verified access via browser.
- Used a curl loop to confirm round-robin distribution between instances

In Bash or Aws console:
while true; do curl -s http://<your-alb-dns-name>; sleep 1; done

✅ Step 4: Create CloudWatch Alarm with SNS Notification
- Set up a CloudWatch alarm on EC2 CPUUtilization metric.
- Configured threshold: CPU > 80% for 5 minutes.
- Created a new SNS topic and subscribed via email to receive alerts.
- Confirmed email subscription to activate notifications.

✅ Step 5: Build a CloudWatch Monitoring Dashboard
- Created a dashboard named web-app-monitoring.
- Added widgets for:
- EC2 CPUUtilization
- ALB request metrics (RequestCount, 5XXErrorCount)
- Optional: Custom text or notes section
- Achieved live observability of system behavior in one unified view.

