# ☁️ Cloud Resume — Hosted on AWS
 
**A static resume deployed on Amazon Web Services using S3 for object storage and CloudFront as a global CDN, demonstrating core AWS infrastructure skills.**
 
🔗 **Live Site:** [d226l6thaolcbq.cloudfront.net](https://d226l6thaolcbq.cloudfront.net)
 
---
 
## Architecture Overview
 
```
┌──────────────┐       ┌────────────────────┐       ┌──────────────────────┐
│              │       │                    │       │                      │
│    User /    │──────▶│   AWS CloudFront   │──────▶│   Amazon S3 Bucket   │
│   Browser    │ HTTPS │   (CDN — Global    │       │ lutabingwa-cloud-    │
│              │◀──────│    Edge Network)   │◀──────│ resume               │
│              │       │                    │       │ (us-east-1)          │
└──────────────┘       └────────────────────┘       └──────────────────────┘
                        Distribution:                Storage Class: Standard
                        E238YOR6L68I9L               Origin for CloudFront
                        d226l6thaolcbq.
                        cloudfront.net
```
 
---
 
## AWS Services Used
 
### Amazon S3 (Simple Storage Service)
- **Bucket:** `lutabingwa-cloud-resume`
- **Region:** `us-east-1` (N. Virginia)
- **Storage class:** Standard
- **Purpose:** Stores the resume PDF and any static assets as S3 objects. The bucket serves as the origin for the CloudFront distribution, meaning CloudFront pulls content from here and caches it at edge locations worldwide.
 
### Amazon CloudFront (Content Delivery Network)
- **Distribution ID:** `E238YOR6L68I9L`
- **Domain:** `d226l6thaolcbq.cloudfront.net`
- **Price class:** All edge locations (best performance)
- **Supported protocols:** HTTP/2, HTTP/1.1, HTTP/1.0
- **Default root object:** `JonathanLutabingwa-Resume1.pdf`
- **Purpose:** Caches and delivers the resume from 450+ edge locations globally, reducing latency for visitors regardless of geographic location. Provides HTTPS encryption in transit and shields the S3 origin from direct public access.
 
---
 
## How It Works
 
1. **Upload** — The resume PDF is uploaded to the S3 bucket `lutabingwa-cloud-resume` as an object.
2. **Origin configuration** — The CloudFront distribution is configured with the S3 bucket as its origin, meaning CloudFront knows where to fetch the source files.
3. **Edge caching** — When a user visits the CloudFront URL, the request is routed to the nearest AWS edge location. If the content is cached there, it's returned immediately. If not, CloudFront fetches it from the S3 origin, caches it, and then serves it.
4. **Global delivery** — Subsequent requests from the same region are served from cache with minimal latency, typically under 50ms.
 
---
 
## Project Structure
 
```
lutabingwa-cloud-resume/          # S3 Bucket
│
├── Jonathan Lutabingwa - Resume (1).pdf   # Resume document
└── (additional assets as needed)
```
 
---
 
## Deployment Steps
 
### 1. Create the S3 Bucket
```bash
aws s3 mb s3://lutabingwa-cloud-resume --region us-east-1
```
 
### 2. Upload the Resume
```bash
aws s3 cp "Jonathan Lutabingwa - Resume.pdf" s3://lutabingwa-cloud-resume/ \
    --storage-class STANDARD
```
 
### 3. Create a CloudFront Distribution
```bash
aws cloudfront create-distribution \
    --origin-domain-name lutabingwa-cloud-resume.s3.us-east-1.amazonaws.com \
    --default-root-object JonathanLutabingwa-Resume1.pdf \
    --comment "lutabingwa-cloud-resume" \
    --enabled \
    --price-class PriceClass_All \
    --http-version http2
```
 
### 4. Invalidate Cache (after updating the resume)
```bash
aws cloudfront create-invalidation \
    --distribution-id E238YOR6L68I9L \
    --paths "/*"
```
 
---
 
## Key AWS Concepts Demonstrated
 
| Concept | How It's Applied |
|---|---|
| **Object storage** | Resume stored as an S3 object with Standard storage class |
| **Global content delivery** | CloudFront distributes the resume across 450+ edge locations |
| **HTTPS / TLS** | CloudFront provides encrypted connections by default |
| **Edge caching** | Repeat visitors are served from cache, reducing latency and S3 request costs |
| **Origin architecture** | S3 bucket configured as the CloudFront origin with a default root object |
| **Price class optimization** | Using all edge locations for maximum global coverage |
| **HTTP/2 support** | Enabled for faster multiplexed connections |
| **AWS CLI proficiency** | Infrastructure can be managed entirely through the CLI |
 
---
 
## Technologies
 
![AWS](https://img.shields.io/badge/AWS-%23FF9900.svg?style=for-the-badge&logo=amazon-aws&logoColor=white)
![Amazon S3](https://img.shields.io/badge/Amazon%20S3-569A31?style=for-the-badge&logo=amazon-s3&logoColor=white)
![CloudFront](https://img.shields.io/badge/CloudFront-8C4FFF?style=for-the-badge&logo=amazon-aws&logoColor=white)
 
---
 
## Author
 
**Jonathan Lutabingwa**
 
---
 
> Built with AWS to demonstrate cloud infrastructure skills. This project is part of an ongoing effort to build real-world experience with Amazon Web Services.
 
