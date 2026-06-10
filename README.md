# Secure Static Website Hosting using Private S3 and CloudFront

## Project Overview

This project demonstrates how to host a secure static website using a private Amazon S3 bucket and Amazon CloudFront with Origin Access Control (OAC).

---

## Architecture

Users → CloudFront → Private S3 Bucket

---

## Prerequisites

* AWS Account
* Basic HTML/CSS Website Files
* IAM User with S3 and CloudFront Permissions

---

## Step 1: Create a Private S3 Bucket

1. Login to AWS Console.
2. Navigate to S3.
3. Click Create Bucket.
4. Enter Bucket Name:

   * secure-static-website
5. Select Region.
6. Keep Block All Public Access enabled.
7. Click Create Bucket.

---

## Step 2: Upload Website Files

Upload:

* index.html
* style.css
* script.js

Navigate:

S3 → Bucket → Upload → Add Files → Upload

---

## Step 3: Create CloudFront Distribution

1. Navigate to CloudFront.
2. Click Create Distribution.
3. Select the S3 bucket as Origin.

Important:

Use the S3 Origin:

secure-static-website.s3.ap-south-1.amazonaws.com

Do NOT use the S3 Website Endpoint.

---

## Step 4: Enable Origin Access Control (OAC)

1. Under Origin Access select:

   * Origin Access Control Settings
2. Click Create New OAC.
3. Configure:

   * Name: Secure-OAC
   * Origin Type: S3
   * Signing Behavior: Sign Requests
4. Save and Attach OAC.

---

## Step 5: Update Bucket Policy

Navigate:

S3 → Bucket → Permissions → Bucket Policy

Add the CloudFront generated policy:

{
"Version": "2012-10-17",
"Statement": [
{
"Sid": "AllowCloudFrontServicePrincipal",
"Effect": "Allow",
"Principal": {
"Service": "cloudfront.amazonaws.com"
},
"Action": "s3:GetObject",
"Resource": "arn:aws:s3:::secure-static-website/*",
"Condition": {
"StringEquals": {
"AWS:SourceArn": "arn:aws:cloudfront::<ACCOUNT_ID>:distribution/<DISTRIBUTION_ID>"
}
}
}
]
}

Save the policy.

---

## Step 6: Configure Default Root Object

Navigate:

CloudFront → Distribution → General → Edit

Set:

Default Root Object: index.html

Save changes.

---

## Step 7: Deploy Distribution

Wait for CloudFront deployment.

Status should become:

Deployed

---

## Step 8: Test Website Access

Copy CloudFront Domain Name.

Example:

https://dxxxxxxxxxxxxx.cloudfront.net

Open the URL.

Expected Result:

Website loads successfully.

---

## Step 9: Verify Direct S3 Access is Blocked

Open:

https://secure-static-website.s3.ap-south-1.amazonaws.com/index.html

Expected Result:

Access Denied

This confirms that the bucket is private and only CloudFront can access the content.

---

## Project Outcome

* Created Private S3 Bucket
* Uploaded Static Website Files
* Configured CloudFront Distribution
* Enabled Origin Access Control (OAC)
* Restricted S3 Access to CloudFront Only
* Verified Secure Website Access
* Blocked Direct S3 Access

---

## Technologies Used

* Amazon S3
* Amazon CloudFront
* Origin Access Control (OAC)
* HTML
* CSS
* AWS IAM
