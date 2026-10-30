# Static Website on AWS with CloudFormation

A simple, secure company landing page for **Myzz Innovate and Tech Solutions**, deployed as Infrastructure as Code with AWS CloudFormation.

**Live site:** https://d3w3ub992mi3r8.cloudfront.net

## Architecture

```
Visitor (HTTPS)
      |
      v
 CloudFront  (CDN, caching, HTTPS)
      |
      |  Origin Access Control (signed requests)
      v
 Private S3 bucket  (index.html, error.html)
```

## AWS services used

| Service | Purpose |
|---------|---------|
| Amazon S3 | Stores the website files in a private bucket |
| Amazon CloudFront | Serves the site over HTTPS and caches it close to visitors |
| CloudFront Origin Access Control | Lets only CloudFront read the private bucket |
| AWS CloudFormation | Creates and updates everything from a template |

## Project files

| File | Description |
|------|-------------|
| `static-site.yaml` | Version 1: S3 website hosting with a public bucket (HTTP only) |
| `static-site-v2.yaml` | Version 2: private S3 bucket behind CloudFront (HTTPS) |
| `index.html` | The company landing page |
| `error.html` | Custom 404 page |

## How it was built

### Version 1: S3 static website
- Created a bucket with website hosting turned on.
- Allowed public read access with a bucket policy.
- Output: the S3 website URL (HTTP only).

### Version 2: CloudFront upgrade
- Made the bucket private by blocking all public access.
- Added an Origin Access Control so CloudFront can read the bucket.
- Added a CloudFront distribution that redirects HTTP to HTTPS and uses `index.html` as the default page.
- Mapped 403 and 404 errors to `error.html`.
- Updated the existing stack instead of creating a new one.

## Deploy it yourself

1. Open the **CloudFormation** console.
2. Click **Create stack** > **With new resources (standard)**.
3. Choose **Upload a template file** and select `static-site-v2.yaml`.
4. Enter a stack name, for example `static-site-stack`.
5. Keep the defaults, click **Next**, then **Submit**.
6. Wait for `CREATE_COMPLETE`.
7. Open the S3 bucket and upload `index.html` and `error.html`.
8. Open the `SiteURL` value from the stack **Outputs** tab.

> **Note:** S3 bucket names are globally unique. Change `BucketName` in the template to your own lowercase name before deploying.

## Clean up

1. Empty the S3 bucket. CloudFormation cannot delete a bucket that contains files.
2. Delete the stack in the CloudFormation console.

## What I learned

- Writing CloudFormation templates with Resources and Outputs
- Using intrinsic functions: `!Ref`, `!Sub` and `!GetAtt`
- Updating a live stack with a direct update, compared with a change set
- Keeping an S3 bucket private and serving it securely through CloudFront
- Why CloudFront caches files, and why changes need an invalidation to appear quickly
- Why a private bucket returns 403 for missing files, so both 403 and 404 map to the error page

## Next ideas

- Add a contact form with API Gateway, Lambda and SNS
- Add a custom domain with Route 53 and an ACM certificate
- Automate deployment with GitHub Actions

## Author

**Erica Gyamfuaa Fordjour (Myzz Rica)**
Founder, Myzz Innovate and Tech Solutions
Ghana
