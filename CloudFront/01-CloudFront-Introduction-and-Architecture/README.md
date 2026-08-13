# AWS CloudFront — Introduction and Architecture

## What is CloudFront?

Amazon CloudFront is a **Content Delivery Network (CDN)** provided by AWS.

A CDN uses globally distributed locations to deliver content closer to users.

Without a CDN:

```text
User → Internet → Origin Server
```

With CloudFront:

```text
User → CloudFront Edge Location → Origin
```

If content is already cached, CloudFront can return it directly from the edge.

## Why CloudFront?

- Low latency
- Faster content delivery
- Reduced origin traffic
- Reduced bandwidth consumption
- Global content distribution
- HTTPS support
- AWS WAF integration
- AWS Shield integration
- S3, ALB, EC2 and API Gateway support

## Important Components

### Distribution

A CloudFront **Distribution** is the main configuration that controls how content is delivered.

### Origin

The **Origin** is where CloudFront retrieves content.

Common origins:

- S3
- Application Load Balancer
- EC2
- API Gateway
- Public HTTP/HTTPS server

### Edge Location

An **Edge Location** is a location where CloudFront can cache and serve content.

```text
                    Origin
                      |
                      v
                 CloudFront
                 /    |    \
                v     v     v
             India  Europe  USA
              Edge   Edge   Edge
                |      |      |
                v      v      v
              Users  Users  Users
```

### Cache Behavior

Cache Behaviors define how CloudFront handles specific paths.

Example:

```text
/*          → S3
/images/*  → S3
/api/*     → ALB
```

### Viewer

The viewer is the client making the request, such as a browser or mobile application.

## CloudFront Request Flow

```text
User
 |
 v
CloudFront
 |
 +---- Cache Hit ----> Return cached content
 |
 +---- Cache Miss ---> Origin
                         |
                         v
                    Get content
                         |
                         v
                    Cache + Return
```

## CloudFront vs S3

S3 stores objects.

CloudFront delivers those objects globally.

```text
S3 → Stores Content
CloudFront → Delivers Content
```

## CloudFront vs Load Balancer

| CloudFront | Load Balancer |
|---|---|
| CDN | Traffic distribution |
| Global edge locations | Regional service |
| Caches content | Primarily distributes requests |
| Can use S3 as origin | Commonly routes to compute targets |
| Reduces origin traffic | Distributes traffic across targets |

They are commonly used together:

```text
User
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
