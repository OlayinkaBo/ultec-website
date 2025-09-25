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

## Setup Instructions

### 1. Clone Repository
```bash
git clone https://github.com/OlayinkaBo/ultec-website.git
cd ultec-website



