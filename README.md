# Create-S3-bucket-inside-in-EC2-using-CLI


“Version”:

aws --version → the CLI version.

EC2 AMI “version” → we query the latest Amazon Linux 2023 AMI via SSM, so you don’t hardcode it.

S3 bucket creation: In us-east-1 you don’t pass --create-bucket-configuration. In any other region, you must.

Create bucket from EC2: Do it the right way—attach an IAM role to the instance (no access keys on the box).



This guide shows:


1. **Install & verify AWS CLI** (Windows, Linux, macOS)
2. **Configure AWS CLI**
3. **Launch an EC2 instance from the CLI** (and how to pick the AMI “version” safely)
4. **Create an S3 bucket**
5. **Create an S3 bucket *from inside EC2*** using an **IAM role** (no hardcoded keys)

---



---

## 1) Install AWS CLI

### 🪟 Windows

#### Option A) Chocolatey (recommended for scriptability)

**```powershell**
# Run PowerShell as Administrator
choco install awscli -y
aws --version
Option B) Official MSI
cmd
Copy
Edit
msiexec.exe /i https://awscli.amazonaws.com/AWSCLIV2.msi
aws --version
Note: AWS CLI v2 bundles its own Python – you do not need to install Python for the CLI to work.

(If you still want Python:)

winget: winget install -e --id Python.Python.3.11

choco: choco install python -y

**🐧 Linux (Ubuntu / Debian)**
bash
Copy
Edit
sudo apt update
sudo apt install unzip -y
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
aws --version
**🍏 macOS**
bash
Copy
Edit
curl "https://awscli.amazonaws.com/AWSCLIV2.pkg" -o "AWSCLIV2.pkg"
sudo installer -pkg AWSCLIV2.pkg -target /
aws --version
(Optionally: brew install awscli)

**2) Configure AWS CLI**
bash
Copy
Edit
aws configure
# AWS Access Key ID [None]: <YOUR_ACCESS_KEY>
# AWS Secret Access Key [None]: <YOUR_SECRET_KEY>
# Default region name [None]: us-east-1   # or ap-south-1 etc.
# Default output format [None]: json
Create extra named profiles (optional):

bash
Copy
Edit
aws configure --profile dev
aws configure --profile prod
**3) Quick Test**
bash
Copy
Edit
aws sts get-caller-identity
You should see your AWS Account, UserId, and ARN.

**4) Create an S3 Bucket (from your PC)**
IMPORTANT:

Bucket names are globally unique.

us-east-1: do not pass --create-bucket-configuration.

Other regions: must pass --create-bucket-configuration.

Example: us-east-1
bash
Copy
Edit
aws s3api create-bucket \
  --bucket mybucketvishwjeetbidkar \
  --region us-east-1
Example: ap-south-1 (Mumbai)
bash
Copy
Edit
aws s3api create-bucket \
  --bucket mybucketvishwjeetbidkar2 \
  --region ap-south-1 \
  --create-bucket-configuration LocationConstraint=ap-south-1
**List buckets:**

aws s3 ls
