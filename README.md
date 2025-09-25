# ULTEC Website - Automated GitHub to AWS Deployment

This project demonstrates automated deployment of the ULTEC website from GitHub to AWS S3 using GitHub Actions.

## Project Details
- **Repository**: [OlayinkaBo/ultec-website](https://github.com/OlayinkaBo/ultec-website)
- **AWS S3 Bucket**: `ultec-bucket`
- **AWS Region**: `us-east-1`
- **Website URL**: http://ultec-bucket.s3-website-us-east-1.amazonaws.com

## Deployment Status
![GitHub Actions](https://github.com/OlayinkaBo/ultec-website/actions/workflows/deploy.yml/badge.svg)

## Architecture


## Setup Instructions

### 1. Clone Repository
```bash
git clone https://github.com/OlayinkaBo/ultec-website.git
cd ultec-website




## Setup Instructions

    Bucket name: ultec-bucket

    Repository name: ultec-website

    GitHub origin: https://github.com/OlayinkaBo/ultec-website.git

    AWS Region: us-east-1

Step 1: Create the Website Files with Your Details
1.1 Create Project Directory
bash

mkdir ultec-website
cd ultec-website

1.2 Create index.html
html

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ULTEC Website - GitHub to AWS Deployment</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="container">
        <header>
            <h1>🚀 ULTEC Website - Successfully Deployed!</h1>
            <p>Automated deployment from GitHub to AWS S3 using GitHub Actions</p>
        </header>
        
        <main>
            <div class="feature-cards">
                <div class="card">
                    <h3>GitHub Repository</h3>
                    <p>Repository: OlayinkaBo/ultec-website</p>
                </div>
                <div class="card">
                    <h3>AWS S3 Bucket</h3>
                    <p>Bucket: ultec-bucket (us-east-1)</p>
                </div>
                <div class="card">
                    <h3>Deployment Status</h3>
                    <p>CI/CD: GitHub Actions → AWS S3</p>
                </div>
            </div>
            
            <div class="deployment-info">
                <h3>Deployment Status: <span id="status">Active ✅</span></h3>
                <p>Bucket: <strong>ultec-bucket</strong> | Region: <strong>us-east-1</strong></p>
                <p>Last deployed: <span id="timestamp"></span></p>
            </div>
        </main>
        
        <footer>
            <p>ULTEC Website | Powered by GitHub Actions + AWS S3</p>
        </footer>
    </div>
    
    <script>
        document.getElementById('timestamp').textContent = new Date().toLocaleString();
    </script>
</body>
</html>

1.3 Create style.css
css

* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    line-height: 1.6;
    color: #333;
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    min-height: 100vh;
}

.container {
    max-width: 1200px;
    margin: 0 auto;
    padding: 20px;
    min-height: 100vh;
    display: flex;
    flex-direction: column;
}

header {
    text-align: center;
    padding: 40px 20px;
    color: white;
}

header h1 {
    font-size: 2.5rem;
    margin-bottom: 10px;
    text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
}

header p {
    font-size: 1.2rem;
    opacity: 0.9;
}

.feature-cards {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    gap: 20px;
    margin: 40px 0;
}

.card {
    background: white;
    padding: 30px;
    border-radius: 10px;
    box-shadow: 0 10px 30px rgba(0,0,0,0.2);
    text-align: center;
    transition: transform 0.3s ease;
}

.card:hover {
    transform: translateY(-5px);
}

.card h3 {
    color: #667eea;
    margin-bottom: 15px;
    font-size: 1.3rem;
}

.deployment-info {
    background: rgba(255,255,255,0.95);
    padding: 30px;
    border-radius: 10px;
    text-align: center;
    margin-top: 20px;
}

#status {
    color: #28a745;
    font-weight: bold;
}

footer {
    margin-top: auto;
    text-align: center;
    padding: 20px;
    color: white;
    opacity: 0.8;
}

@media (max-width: 768px) {
    header h1 {
        font-size: 2rem;
    }
    
    .feature-cards {
        grid-template-columns: 1fr;
    }
}

1.4 Create GitHub Actions Workflow Directory
bash

mkdir -p .github/workflows

1.5 Create .github/workflows/deploy.yml (Updated with your details)
yaml

name: Deploy ULTEC Website to AWS S3

on:
  push:
    branches: [ main, master ]
  pull_request:
    branches: [ main, master ]

jobs:
  test-deploy:
    runs-on: ubuntu-latest
    
    steps:
    - name: Checkout code
      uses: actions/checkout@v4
      
    - name: Validate website files
      run: |
        echo "🔍 Checking website files..."
        if [ ! -f "index.html" ]; then
          echo "❌ ERROR: index.html not found!"
          exit 1
        fi
        
        if [ ! -f "style.css" ]; then
          echo "❌ ERROR: style.css not found!"
          exit 1
        fi
        
        echo "✅ All required files found!"
        echo "📁 Files in repository:"
        ls -la

  deploy:
    needs: test-deploy
    runs-on: ubuntu-latest
    if: github.event_name == 'push' && (github.ref == 'refs/heads/main' || github.ref == 'refs/heads/master')
    
    steps:
    - name: Checkout code
      uses: actions/checkout@v4
      
    - name: Configure AWS credentials
      uses: aws-actions/configure-aws-credentials@v3
      with:
        aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
        aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        aws-region: us-east-1
        
    - name: Deploy to S3 bucket
      run: |
        echo "🚀 Starting deployment to S3 bucket: ultec-bucket"
        echo "📍 AWS Region: us-east-1"
        
        # Sync files to S3
        aws s3 sync . s3://ultec-bucket \
          --exclude '.git/*' \
          --exclude '.github/*' \
          --exclude 'README.md' \
          --acl public-read \
          --delete
        
        echo "✅ Files uploaded successfully!"
        
    - name: Verify deployment
      run: |
        echo "🔍 Verifying deployment..."
        aws s3 ls s3://ultec-bucket/ --recursive --human-readable --summarize
        
    - name: Display website URL
      run: |
        echo "🎉 Deployment completed successfully!"
        echo "🌐 Your ULTEC website is now live at:"
        echo "   http://ultec-bucket.s3-website-us-east-1.amazonaws.com"
        echo ""
        echo "📊 Repository: https://github.com/OlayinkaBo/ultec-website"
        echo "📍 S3 Bucket: ultec-bucket (us-east-1)"

1.6 Create README.md (Updated with your details)
markdown

# ULTEC Website - Automated GitHub to AWS Deployment

This project demonstrates automated deployment of the ULTEC website from GitHub to AWS S3 using GitHub Actions.

## Project Details
- **Repository**: [OlayinkaBo/ultec-website](https://github.com/OlayinkaBo/ultec-website)
- **AWS S3 Bucket**: `ultec-bucket`
- **AWS Region**: `us-east-1`
- **Website URL**: http://ultec-bucket.s3-website-us-east-1.amazonaws.com

## Deployment Status
![GitHub Actions](https://github.com/OlayinkaBo/ultec-website/actions/workflows/deploy.yml/badge.svg)

## Architecture

GitHub Repository → GitHub Actions → AWS S3 → Website
text


## Setup Instructions

### 1. Clone Repository
```bash
git clone https://github.com/OlayinkaBo/ultec-website.git
cd ultec-website

2. AWS Configuration

    S3 Bucket: ultec-bucket

    Region: us-east-1

    Static website hosting enabled

3. GitHub Secrets Required

    AWS_ACCESS_KEY_ID

    AWS_SECRET_ACCESS_KEY

Manual Deployment Test
bash

git add .
git commit -m "Test deployment"
git push origin main

Website Features

    Responsive design

    Automated CI/CD pipeline

    Zero-downtime deployments

    Real-time deployment status

Access Points

    GitHub Repository: https://github.com/OlayinkaBo/ultec-website

    Live Website: http://ultec-bucket.s3-website-us-east-1.amazonaws.com

    AWS S3 Console: https://s3.console.aws.amazon.com/s3/buckets/ultec-bucket

text


## Step 2: Set Up AWS Infrastructure for ULTEC Bucket

### 2.1 Create S3 Bucket - `ultec-bucket`

1. **Log in to AWS Console**
   - Go to [AWS Management Console](https://console.aws.amazon.com)
   - Sign in with your account

2. **Create S3 Bucket**
   - Navigate to S3 service
   - Click "Create bucket"
   - **Bucket name**: `ultec-bucket`
   - **Region**: `us-east-1` (US East (N. Virginia))
   - Uncheck "Block all public access" and acknowledge the warning
   - Click "Create bucket"

3. **Configure Bucket for Static Website Hosting**
   - Click on `ultec-bucket`
   - Go to **Properties** tab
   - Scroll to **Static website hosting**
   - Click **Edit**
   - Select **Enable**
   - **Index document**: `index.html`
   - **Error document**: `index.html`
   - Click **Save changes**

4. **Set Bucket Policy** (Replace with your exact bucket ARN)
   - Go to **Permissions** tab
   - Scroll to **Bucket policy**
   - Click **Edit** and add:
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::ultec-bucket/*"
        }
    ]
}

Step 3: Set Up GitHub Repository for ULTEC Website
3.1 Initialize Git Repository
bash

# In your project directory
git init
git add .
git commit -m "Initial commit: ULTEC Website with GitHub Actions deployment"

3.2 Create GitHub Repository

    Go to GitHub.com

    Click + → New repository

    Repository name: ultec-website

    Description: "ULTEC Website - Automated deployment from GitHub to AWS"

    Choose Public or Private

    Don't initialize with README (we already have one)

    Click Create repository

3.3 Push Code to GitHub (Updated with your origin)
bash

git branch -M main
git remote add origin https://github.com/OlayinkaBo/ultec-website.git
git push -u origin main

3.4 Add AWS Secrets to GitHub

    Go to your repository: https://github.com/OlayinkaBo/ultec-website

    Click Settings tab

    In left sidebar, click Secrets and variables → Actions

    Click New repository secret

Add these secrets:

    Name: AWS_ACCESS_KEY_ID

        Value: [Your AWS Access Key ID]

    Name: AWS_SECRET_ACCESS_KEY

        Value: [Your AWS Secret Access Key]

Note: We removed AWS_REGION and S3_BUCKET_NAME secrets since they're now hardcoded in the workflow.
Step 4: Test the Deployment
4.1 Trigger Deployment
bash

# Make a small change to test
echo "<!-- ULTEC Website Deployment Test -->" >> index.html
git add index.html
git commit -m "Test deployment for ULTEC website"
git push origin main

4.2 Monitor Deployment

    Go to your GitHub repository: https://github.com/OlayinkaBo/ultec-website

    Click Actions tab

    You should see a workflow running called "Deploy ULTEC Website to AWS S3"

    Click on it to see real-time logs

4.3 Access Your ULTEC Website

After successful deployment, your website will be available at:
text

http://ultec-bucket.s3-website-us-east-1.amazonaws.com

Step 5: Verify and Troubleshoot
5.1 Check S3 Bucket Contents

    Go to AWS S3 Console

    Click on ultec-bucket

    Verify files are uploaded: index.html, style.css

5.2 Test Website URL

Open in your browser: http://ultec-bucket.s3-website-us-east-1.amazonaws.com
5.3 Common Issues and Solutions

Issue: Bucket name already exists

    Solution: S3 bucket names must be globally unique. If ultec-bucket is taken, add numbers/suffix

Issue: 403 Forbidden

    Solution: Verify bucket policy allows public read access

Issue: GitHub Actions failing on AWS credentials

    Solution: Double-check AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY in GitHub secrets

Step 6: Final Verification Commands
bash

# Check your remote origin is correct
git remote -v

# Should show:
# origin  https://github.com/OlayinkaBo/ultec-website.git (fetch)
# origin  https://github.com/OlayinkaBo/ultec-website.git (push)

# Check your files are ready
ls -la

# Should show: index.html, style.css, .github/workflows/deploy.yml, README.md

# Deploy any changes
git add .
git commit -m "Deploy ULTEC website"
git push origin main

Your Specific URLs:

    GitHub Repository: https://github.com/OlayinkaBo/ultec-website

    Live Website: http://ultec-bucket.s3-website-us-east-1.amazonaws.com

    GitHub Actions: https://github.com/OlayinkaBo/ultec-website/actions

The deployment is now fully configured with your specific details! Every push to the main branch will automatically deploy to your S3 bucket.

