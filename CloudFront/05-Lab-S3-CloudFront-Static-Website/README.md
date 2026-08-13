# Lab — Host a Static Website Using S3 and CloudFront

## Objective

In this lab you will:

- Create an S3 bucket
- Upload a static website
- Keep S3 private
- Create a CloudFront distribution
- Configure OAC
- Configure HTTPS
- Test CloudFront caching
- Perform cache invalidation

## Architecture

```text
                    Internet
                       |
                       v
                  CloudFront
                       |
                       | OAC
                       v
                Private S3 Bucket
                       |
              +--------+--------+
              |        |        |
              v        v        v
          index.html  CSS       JS
```

## Step 1 — Create an S3 Bucket

Create a unique S3 bucket.

Example:

```text
bubu-cloudfront-lab-12345
```

Keep the bucket private.

## Step 2 — Create Website Files

Create `index.html`:

```html
<!DOCTYPE html>
<html>
<head>
    <title>CloudFront Lab</title>
</head>
<body>
    <h1>Hello from AWS CloudFront!</h1>
    <p>This website is delivered using Amazon CloudFront.</p>
</body>
</html>
```

Optional `style.css`:

```css
body {
    font-family: Arial, sans-serif;
}

h1 {
    font-size: 40px;
}
```

## Step 3 — Upload Files

Upload:

```text
index.html
style.css
```

to the S3 bucket.

## Step 4 — Create CloudFront Distribution

Go to:

```text
AWS Console
→ CloudFront
→ Distributions
→ Create Distribution
```

Select the S3 bucket as the origin.

## Step 5 — Configure OAC

Create an **Origin Access Control** for the S3 origin.

Allow CloudFront to update the S3 bucket policy when the console provides that option.

Desired flow:

```text
User
 |
 v
CloudFront
 |
 | OAC
 v
Private S3
```

## Step 6 — Configure Default Root Object

Set:

```text
index.html
```

as the default root object.

## Step 7 — Configure HTTPS

Use:

```text
Redirect HTTP to HTTPS
```

## Step 8 — Create the Distribution

Wait until the distribution is deployed.

CloudFront will provide a domain similar to:

```text
https://d123456abcdef.cloudfront.net
```

## Step 9 — Test the Website

Open the CloudFront domain.

Expected result:

```text
Hello from AWS CloudFront!
This website is delivered using Amazon CloudFront.
```

## Step 10 — Test Caching

Modify `index.html`:

```html
<h1>Version 2 of the website</h1>
```

Upload it to S3.

Depending on the cache configuration, CloudFront may continue returning the old version.

This demonstrates caching.

## Step 11 — Create an Invalidation

Go to:

```text
CloudFront
→ Distribution
→ Invalidations
→ Create Invalidation
```

Use:

```text
/*
```

Or invalidate only:

```text
/index.html
```

After the invalidation completes, refresh the CloudFront URL.

The updated content should be displayed.

## Step 12 — AWS CLI

List distributions:

```bash
aws cloudfront list-distributions
```

Get distribution details:

```bash
aws cloudfront get-distribution   --id DISTRIBUTION_ID
```

Create an invalidation:

```bash
aws cloudfront create-invalidation   --distribution-id DISTRIBUTION_ID   --paths "/*"
```

List invalidations:

```bash
aws cloudfront list-invalidations   --distribution-id DISTRIBUTION_ID
```

## Step 13 — Test Response Headers

```bash
curl -I https://YOUR_CLOUDFRONT_DOMAIN
```

Inspect the response headers while troubleshooting caching.

## Lab Validation Checklist

- [ ] S3 bucket created
- [ ] S3 bucket is private
- [ ] Website files uploaded
- [ ] CloudFront distribution created
- [ ] S3 configured as origin
- [ ] OAC configured
- [ ] S3 bucket policy allows CloudFront access
- [ ] `index.html` configured as default root object
- [ ] HTTPS configured
- [ ] Website accessible through CloudFront
- [ ] Caching tested
- [ ] Invalidation performed
- [ ] Updated content verified

## Expected Final Architecture

```text
                         Internet
                            |
                            v
                       CloudFront
                            |
                            | OAC
                            v
                    Private S3 Bucket
                            |
                 +----------+----------+
                 |          |          |
                 v          v          v
             index.html  style.css   images
```
