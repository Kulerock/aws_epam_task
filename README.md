# My website published in S3 AWS
![image](https://github.com/user-attachments/assets/2ad3f639-a204-425b-96c8-c1c46b26950f)

My 2023 CV via AWS [link](https://old-cv-kulerock.s3.eu-north-1.amazonaws.com/index.html)

## How to do it?

Here’s a step-by-step guide to publishing a website on AWS S3 with GitHub integration.

### Step 1: Setting Up a GitHub Repository
First, I created a private repository on GitHub to store my website files securely. This allows me to manage my content versioning and keep files accessible only to me or specific collaborators.
- **Why GitHub?** It provides a reliable and secure platform for code and asset management, and it’s essential for integrating GitHub Actions, which automates our deployment.
![image](https://github.com/user-attachments/assets/2098f673-2c5e-4082-9d21-93f3ce73280b)

### Step 2: AWS IAM User Creation

I created a dedicated IAM user in AWS with permissions for S3. This user will securely manage access to my bucket.

- **Go to IAM:** Navigate to the IAM service in AWS.
- **Create User:** Add a new user with a custom name and assign `AmazonS3FullAccess` permissions.
- **Save Access Key:** Make sure to securely store the access key for this user, as it will be used later in GitHub.

![image](https://github.com/user-attachments/assets/4e8cc0db-c7bc-46ec-a633-dea9dac9f1b5)
![image](https://github.com/user-attachments/assets/624945d2-71d3-479a-86db-c02831ba2153)
![image](https://github.com/user-attachments/assets/e87b57dd-721f-4303-90f6-a3ca076544c7)

#### Step 3: Configuring an S3 Bucket with Public Access

To host the website, I created an S3 bucket with public access. This bucket will store my static files and serve them as a website.

- **Create Bucket:** Go to the S3 service and create a new bucket. Name it, and select the region.
- **Enable Public Access:** Uncheck "Block all public access" so the website can be publicly accessible.
- **Set Bucket Policy:** Configure the bucket policy to allow public `s3:GetObject` permissions for everyone to access the files.

![image](https://github.com/user-attachments/assets/533b9205-3529-4e4e-a2ad-7d779be354ea)
![image](https://github.com/user-attachments/assets/e3932697-ba41-40ec-af79-179f102646b0)
![image](https://github.com/user-attachments/assets/a778d400-85b3-4792-ac55-5071ebc3629d)
![image](https://github.com/user-attachments/assets/f2838063-a1fd-4e2f-a503-17128f83a2ee)

### Step 4: Adding GitHub Secrets for Secure Access

In GitHub, I added the AWS access keys as secrets to connect my GitHub Actions to AWS securely.

- **Go to GitHub Settings:** Open the repository, go to Settings > Secrets and variables > Actions.
- **Add Secrets:** Create two secrets, `AWS_ROLE_ACCESS_KEY` and `AWS_ROLE_SECRET_KEY`, and paste in the values from IAM.
![image](https://github.com/user-attachments/assets/27754c1a-9100-423c-9777-2ed5162623d8)
![image](https://github.com/user-attachments/assets/5e78e018-23e3-45b2-b114-28aee5e70294)

### Step 5: Creating a GitHub Workflow for Automatic Deployment

Using GitHub Actions, I created a workflow to automatically upload files to S3 whenever I push new changes to the main branch.

- **Set Up Workflow:** Go to Actions in your repository, choose "New workflow," and select "set up a workflow yourself."
- **Configure Script:** Add the script to specify the workflow steps, including checking out the repository and uploading files to S3.
- 
![image](https://github.com/user-attachments/assets/e8bba122-0fb7-429e-9c02-5aadb8031a36)

**Add this code:**
```yaml
name: Upload Website

on:
  push:
    branches:
    - main

jobs:
  deploy-site:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: jakejarvis/s3-sync-action@master
        with:
          args: --delete
        env:
          AWS_S3_BUCKET: YOUR_BUCKET_NAME
          AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
          AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          SOURCE_DIR: ./main
```

### Step 6: Running GitHub Action to Deploy

With each push to the main branch, the GitHub Action automatically deploys updates to my S3 bucket.

- **Commit Changes:** Each commit to the `main` branch triggers the deployment workflow.
- **Monitor Workflow:** Go to the **Actions** tab in GitHub to monitor the workflow’s progress and ensure the successful deployment.
- 
![image](https://github.com/user-attachments/assets/db478da4-c95e-4d1b-a7de-b31a5ff2d92f)

### Step 7: Verifying the Deployment

Finally, I checked the S3 bucket to ensure the website files were deployed successfully and accessible via the S3 URL.

![image](https://github.com/user-attachments/assets/68c6444b-0db5-46ae-9335-dd6590eba007)

And that’s it! My website is now live, hosted securely on AWS S3.




