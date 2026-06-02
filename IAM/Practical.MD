Step 1: Create a IAM user named “Mayuri” assign “iamfullaccess policy document” from GUI, download the excel.
Step 2: login with “Mayuri” form aws console and try to create ec2 Instance.
Step 3: Assign EC2Fullaccess policy document to the user “Mayuri” and try to create EC2 instance with tag Key=Name Value=webserver
Step 4: Create a role with name “manishaiamadmin” and assign iamfullaccess policy document to the role.
Step 5: Assign the role to running instance “webserver”.
Step 6: Login to instance with public IP and private key.
When someone says “Login to instance with public IP and private key”, they’re usually talking about connecting to a cloud server (like an AWS EC2 instance, Azure VM, or similar) using SSH (Secure Shell).
Step 7: Create an aws iam user from the instance (we will be able to create an iam user because role is attached).
Step 8: Follow the steps given in slides 14th 15th & 16th.
Step 9: Remove role “iamrole” from the instance “webserver” which is attached.
Step 10: Now try to create iam user again, you will not be able to create it as the role has been removed.
Step 11: Configure iam aws user to work from CLI.
Use Create IAM User “Mayuri”
Get Keys → Download the Access Key ID and Secret Access Key.
Install AWS CLI → Ensure the CLI is installed on your system.
Run aws configure → Enter the keys, default region, and output format.
Verify → Use aws sts get-caller-identity to confirm the setup.
Use CLI → Run commands like aws s3 ls or aws ec2 describe-instances.
