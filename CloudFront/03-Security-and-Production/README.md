# AWS CloudFront — Security and Production

## Origin Access Control

When using S3, keep the bucket private and allow CloudFront to access it using **Origin Access Control (OAC)**.

Recommended architecture:

```text
Internet
   |
   v
CloudFront
   |
   | OAC
   v
Private S3
```

## HTTPS

CloudFront supports HTTPS using SSL/TLS certificates.

For a custom domain:

```text
www.example.com
       |
       v
   CloudFront
       |
       v
     Origin
```

AWS Certificate Manager (ACM) can be used to manage the certificate.

## Viewer Protocol Policy

Common options:

- HTTP and HTTPS
- Redirect HTTP to HTTPS
- HTTPS only

For production, HTTPS should normally be preferred.

## AWS WAF

CloudFront integrates with AWS WAF:

```text
Internet
   |
   v
CloudFront
   |
   v
AWS WAF
   |
   v
Origin
```

WAF can provide:

- IP-based rules
- Rate-based rules
- Managed rule groups
- Request filtering
- Geographic controls

## DDoS Protection

CloudFront integrates with AWS Shield Standard.

For additional DDoS protection requirements, AWS Shield Advanced can be considered.

## Geo Restriction

CloudFront can restrict access based on geographic location.

## Monitoring

CloudFront integrates with CloudWatch.

Useful metrics include:

- Requests
- Bytes downloaded
- Bytes uploaded
- 4xx errors
- 5xx errors
- Cache hit rate

## Production Architecture

```text
                         Internet
                            |
                            v
                        Route 53
                            |
                            v
                       CloudFront
                            |
                         AWS WAF
                            |
                    +-------+-------+
                    |               |
                    v               v
                   S3              ALB
                                    |
                              +-----+-----+
                              |     |     |
                              v     v     v
                             EC2   EC2   EC2
```

## Production Best Practices

1. Use HTTPS.
2. Use OAC for private S3 origins.
3. Configure cache policies carefully.
4. Use versioned static assets.
5. Integrate WAF where appropriate.
6. Monitor cache hit ratio and errors.
7. Use Route 53 and ACM for custom domains.
8. Avoid caching sensitive or dynamic content incorrectly.


## CloudFront + S3

A common static website architecture is:

```text
User
 |
 v
CloudFront
 |
 | OAC
 v
Private S3
 |
 +---- index.html
 +---- style.css
 +---- script.js
 +---- images/
```

S3 stores the content while CloudFront delivers it globally.

## CloudFront + ALB

For an application running on EC2:

```text
Users
  |
  v
CloudFront
  |
  v
ALB
  |
  +---- EC2
  +---- EC2
  +---- EC2
```

CloudFront provides the global edge layer while ALB distributes requests across application targets.

## CloudFront + EKS

A production-style EKS architecture can look like:

```text
Internet
   |
   v
CloudFront
   |
   v
ALB
   |
   v
Ingress
   |
   v
Service
   |
   v
Pods
```

CloudFront can reduce traffic reaching the cluster when content is cacheable.

## Static + Dynamic Application

CloudFront can route different paths to different origins:

```text
CloudFront
    |
    +---- /static/* → S3
    |
    +---- /api/*    → ALB → EKS
```

## Important Considerations

- Do not blindly cache API responses.
- Configure cache behaviors carefully.
- Allow only required HTTP methods.
- Use HTTPS.
- Secure the origin.
- Monitor CloudFront, ALB and application metrics.
