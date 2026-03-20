# Cloud Resume — Hosted on AWS
 
Deployed a static resume on AWS using S3 for object storage and CloudFront as a global CDN. This was my first AWS project to get hands-on with core cloud infrastructure.
 
🔗 **Live Site:** [d226l6thaolcbq.cloudfront.net](https://d226l6thaolcbq.cloudfront.net)
 
> Part of my [AWS Cloud Projects](https://github.com/jtlutabingwa/AWS-Cloud-Projects) collection.
 
## Overview
 
| | |
|---|---|
| **Services Used** | S3, CloudFront |
| **Region** | us-east-1 (N. Virginia) |
| **Delivery** | 450+ edge locations globally |
| **Cost** | Free Tier eligible |

 ## Key Concepts
 
| Concept | What It Is |
|---|---|
| **S3** | Object storage service — stores files as objects in buckets |
| **CloudFront** | Content delivery network — caches and serves content from 450+ global edge locations |
| **Origin** | The source where CloudFront pulls content from (in this case, the S3 bucket) |
| **Edge Location** | A data center in CloudFront's network closest to the user |
| **Cache Invalidation** | Forces CloudFront to fetch fresh content from the origin instead of serving stale cache |
| **HTTPS/TLS** | Encryption in transit — CloudFront provides this by default |
| **HTTP/2** | Faster protocol with multiplexed connections, enabled on the distribution |

## Architecture
 
```
User (Browser)
     │
     │  HTTPS
     ▼
┌──────────────────────┐
│   AWS CloudFront     │  ← CDN, caches content at edge locations
│   (Global Edge       │
│    Network)          │
└──────────┬───────────┘
           ▼
┌──────────────────────┐
│   Amazon S3 Bucket   │  ← Stores the resume PDF
│   lutabingwa-cloud-  │
│   resume             │
└──────────────────────┘
```
 
## What I Did
 
### Created the S3 Bucket
 
Set up a bucket called `lutabingwa-cloud-resume` in us-east-1 with Standard storage class. Uploaded the resume PDF as an S3 object.
 
```bash
aws s3 mb s3://lutabingwa-cloud-resume --region us-east-1
aws s3 cp "Jonathan Lutabingwa - Resume.pdf" s3://lutabingwa-cloud-resume/
```
 
### Set Up CloudFront Distribution
 
Created a CloudFront distribution pointing to the S3 bucket as its origin. This caches the resume at edge locations worldwide so it loads fast regardless of where someone is.
 
```bash
aws cloudfront create-distribution \
    --origin-domain-name lutabingwa-cloud-resume.s3.us-east-1.amazonaws.com \
    --default-root-object JonathanLutabingwa-Resume1.pdf \
    --enabled \
    --price-class PriceClass_All \
    --http-version http2
```
 
### Cache Invalidation
 
When I update the resume, I invalidate the CloudFront cache so the new version gets served immediately instead of waiting for the old cache to expire.
 
```bash
aws cloudfront create-invalidation \
    --distribution-id E238YOR6L68I9L \
    --paths "/*"
```
 
## What I Learned
 
- S3 stores files as objects in buckets — it's not a traditional file system
- CloudFront caches content at edge locations so users get served from the nearest one
- CloudFront provides HTTPS by default, so the resume is encrypted in transit
- Cache invalidation is needed after updating files, otherwise CloudFront keeps serving the old version
- The entire project can be managed through the AWS CLI without touching the console
