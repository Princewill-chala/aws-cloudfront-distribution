# AWS CloudFront Distribution with Amazon S3

![Project Banner](images/cloudfront_banner.png)

## 📌 Project Overview

This project demonstrates how to deploy a secure content delivery solution using **Amazon CloudFront** and **Amazon S3**.

Instead of making the S3 bucket publicly accessible, Amazon CloudFront is configured to securely retrieve content from the bucket using **Origin Access Control (OAC)**. This approach improves security while allowing users to access website content through CloudFront's global Content Delivery Network (CDN).

During this hands-on project, a private S3 bucket was created, HTML objects were uploaded, and a CloudFront distribution was configured to serve the website securely. Direct access to the S3 objects resulted in an **Access Denied** error, while accessing the same objects through the CloudFront distribution succeeded.

---

## 🎯 Project Objectives

- Create a private Amazon S3 bucket.
- Upload static website content.
- Configure an Amazon CloudFront distribution.
- Secure the S3 bucket using Origin Access Control (OAC).
- Verify secure content delivery through CloudFront.
- Understand why direct S3 access is blocked while CloudFront access is permitted.

---

## 🛠 AWS Services Used

- Amazon CloudFront
- Amazon S3
- Origin Access Control (OAC)
- IAM Policy (Bucket Policy)

---

## 📁 Project Architecture

```text
                    +----------------------+
                    |      End Users       |
                    +----------+-----------+
                               |
                               |
                     HTTPS Request
                               |
                               ▼
                  +-----------------------+
                  |   Amazon CloudFront   |
                  |   Distribution (CDN)  |
                  +-----------+-----------+
                              |
                  Origin Access Control
                              |
                              ▼
                  +-----------------------+
                  |     Amazon S3 Bucket  |
                  | demo-cloudfront-      |
                  | chala-v1 (Private)    |
                  +-----------------------+
                              |
                    Static Website Files
                  (HTML, CSS, Images, JS)
```

---

## 📸 Project Screenshots

| Step | Screenshot |
|------|------------|
| CloudFront Service | ![](images/cloudfront.png) |
| S3 Bucket Created | ![](images/s3_bucket.png) |
| Objects Uploaded | ![](images/objects_upload.png) |
| Create Distribution | ![](images/create_distribution.png) |
| Distribution Created | ![](images/distribution.png) |
| Bucket Policy | ![](images/s3_bucket_policy.png) |
| Access Denied | ![](images/access_denied.png) |

--- 

## 🚀 Implementation Steps

### Step 1: Create an Amazon S3 Bucket

Create a private Amazon S3 bucket that will store the static website files.

**Bucket Name**

```text
demo-cloudfront-chala-v1
```

![](images/s3_bucket.png)

---

### Step 2: Upload Website Objects

Upload the website files into the S3 bucket.

Example files:

- index.html
- images
- CSS files
- JavaScript files

Since the bucket is private, attempting to open the HTML file directly from Amazon S3 returns an **Access Denied** error.

![](images/objects_upload.png)

---

### Step 3: Verify Private Bucket Access

Open the uploaded HTML object directly from Amazon S3.

Because the bucket does **not** allow public access, Amazon S3 blocks the request.

This confirms that the website is protected from direct public access.

![](images/access_denied.png)

---

### Step 4: Create a CloudFront Distribution

Navigate to **Amazon CloudFront** and create a new distribution.

Configuration used during this project:

| Setting | Value |
|---------|-------|
| Distribution Name | DemoCloudFront |
| Origin Type | Amazon S3 |
| Origin | demo-cloudfront-chala-v1 |
| Security | Default (Origin Access Control) |

![](images/cloudfront.png)

![](images/create_distribution.png)

---

### Step 5: Review the Generated Bucket Policy

After creating the CloudFront distribution, Amazon automatically generated a bucket policy allowing **only CloudFront** to retrieve objects from the private S3 bucket.

This ensures that users cannot bypass CloudFront and access objects directly from Amazon S3.

![](images/s3_bucket_policy.png)

---

### Step 6: Verify CloudFront Distribution

Once deployment completed, open the CloudFront Distribution.

Initially, browsing only the distribution domain displays an **Access Denied** response because CloudFront is requesting the bucket root.

Appending the object path (for example, `/index.html`) to the CloudFront URL successfully loads the website.

Example:

```text
https://xxxxxxxxxxxxx.cloudfront.net/index.html
```

![](images/distribution.png)

---

## ✅ Result

The static website is now delivered through Amazon CloudFront while the Amazon S3 bucket remains private.

This architecture provides:

- Secure content delivery
- Global low-latency access
- Improved performance through caching
- Protection against direct S3 object access
- AWS best-practice implementation using Origin Access Control (OAC)

---

---

## 🔐 S3 Bucket Policy

To allow Amazon CloudFront to securely retrieve objects from the private Amazon S3 bucket, AWS automatically generated the following bucket policy using **Origin Access Control (OAC)**.

This policy grants CloudFront permission to perform the `s3:GetObject` action while preventing direct public access to the bucket.

```json
{
    "Version": "2008-10-17",
    "Id": "PolicyForCloudFrontPrivateContent",
    "Statement": [
        {
            "Sid": "AllowCloudFrontServicePrincipal",
            "Effect": "Allow",
            "Principal": {
                "Service": "cloudfront.amazonaws.com"
            },
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::your-bucket-name/*",
            "Condition": {
                "ArnLike": {
                    "AWS:SourceArn": "arn:aws:cloudfront::<ACCOUNT_ID>:distribution/<DISTRIBUTION_ID>"
                }
            }
        }
    ]
}
```

---

## 💡 Key Concepts Learned

Throughout this project, the following AWS concepts were explored:

- Amazon CloudFront as a Content Delivery Network (CDN)
- Amazon S3 as a secure origin for static website hosting
- Origin Access Control (OAC)
- CloudFront Distribution creation
- CloudFront Origin configuration
- Bucket Policies
- Secure object access
- Content delivery through Edge Locations
- Access control using IAM resource policies

---

## 🏆 Skills Gained

By completing this project, I gained hands-on experience with:

- Creating Amazon S3 buckets
- Uploading static website content
- Configuring Amazon CloudFront Distributions
- Securing S3 buckets using Origin Access Control (OAC)
- Understanding CloudFront request routing
- Working with Bucket Policies
- Troubleshooting **Access Denied** errors
- Delivering content securely using AWS global edge locations

---

## 📖 What I Learned

Some important lessons from this project include:

- Amazon S3 buckets should remain private whenever possible.
- CloudFront provides secure, high-performance content delivery.
- Origin Access Control (OAC) is the recommended method for allowing CloudFront to access private S3 buckets.
- Direct access to S3 objects is blocked unless explicitly permitted.
- CloudFront caches content at edge locations to reduce latency and improve user experience.
- Secure architectures are preferred over making storage buckets publicly accessible.

---

## 🚀 Future Improvements

Possible enhancements for this project include:

- Register a custom domain using Amazon Route 53
- Secure the website with HTTPS using AWS Certificate Manager (ACM)
- Configure custom error pages
- Enable CloudFront access logging
- Integrate AWS WAF for enhanced security
- Automate deployment using AWS CloudFormation or Terraform
- Build a CI/CD pipeline for automatic website deployments

---

## 🎯 Conclusion

This project demonstrates how Amazon CloudFront can securely distribute static website content stored in a private Amazon S3 bucket.

Using **Origin Access Control (OAC)** ensures that only CloudFront can access the bucket, eliminating the need to expose S3 objects publicly. This architecture follows AWS security best practices while providing fast, reliable, and globally distributed content delivery.

---

## 👨‍💻 Author

**Elochukwu Princewill**

Cloud | Cybersecurity | AWS Solutions Architecture | Linux | Networking

> *Learning by building real-world cloud infrastructure projects.*

---

## ⭐ If you found this project helpful

Feel free to ⭐ star this repository and explore my other AWS hands-on projects as I continue building my cloud engineering portfolio.