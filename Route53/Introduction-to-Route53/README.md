# AWS Route 53 – Complete Study Notes

## 📘 Introduction

AWS Route 53 is a highly available and scalable **Domain Name System (DNS)** web service provided by AWS.

It is used to:

- Register domain names
- Route internet traffic
- Perform DNS resolution
- Monitor application health
- Route users to healthy endpoints

AWS Route 53 helps users access applications using human-readable domain names instead of IP addresses.

Example:

```text
www.example.com
```

instead of:

```text
192.168.1.10
```

---

# 📋 Table of Contents

- [What is DNS?](#-what-is-dns)
- [What is Route 53?](#-what-is-route-53)
- [Why is it Called Route 53?](#-why-is-it-called-route-53)
- [How DNS Works](#-how-dns-works)
- [Key Features](#-key-features)
- [Core Components](#-core-components)
- [Hosted Zones](#-hosted-zones)
- [DNS Records](#-dns-records)
- [Routing Policies](#-routing-policies)
- [Health Checks](#-health-checks)
- [Domain Registration](#-domain-registration)
- [Public vs Private Hosted Zones](#-public-vs-private-hosted-zones)
- [Alias Records](#-alias-records)
- [TTL (Time To Live)](#-ttl-time-to-live)
- [DNS Resolution Flow](#-dns-resolution-flow)
- [Route 53 with AWS Services](#-route-53-with-aws-services)
- [Security](#-security)
- [Pricing](#-pricing)
- [Advantages](#-advantages)
- [Disadvantages](#-disadvantages)
- [Best Practices](#-best-practices)
- [Interview Questions](#-interview-questions)
- [Real-World Use Cases](#-real-world-use-cases)
- [Conclusion](#-conclusion)

---

# 🌐 What is DNS?

DNS stands for:

```text
Domain Name System
```

DNS translates domain names into IP addresses.

Example:

```text
google.com → 142.250.x.x
```

Humans remember names easily, while computers communicate using IP addresses.

DNS acts like the internet’s phonebook.

---

# 🚀 What is Route 53?

AWS Route 53 is a managed DNS service used for:

- DNS management
- Traffic routing
- Domain registration
- Health monitoring

It is highly available and globally distributed.

---

# ❓ Why is it Called Route 53?

The name comes from:

```text
Port 53
```

Port 53 is used for DNS services.

Both:

- TCP Port 53
- UDP Port 53

are used in DNS communication.

---

# 🔄 How DNS Works

When a user enters:

```text
www.example.com
```

DNS performs these steps:

```text
User Browser
   ↓
DNS Resolver
   ↓
Root DNS Server
   ↓
TLD Server (.com)
   ↓
Authoritative DNS Server
   ↓
IP Address Returned
```

Finally, the browser connects to the server using the IP address.

---

# ✨ Key Features

## 1. Highly Available

Route 53 is globally distributed and fault tolerant.

---

## 2. Scalable

Handles millions of DNS queries automatically.

---

## 3. Domain Registration

You can purchase and manage domains directly in Route 53.

Example:

```text
mywebsite.com
```

---

## 4. Health Checks

Monitors endpoint health and redirects traffic if unhealthy.

---

## 5. Traffic Routing

Supports advanced routing policies such as:

- Weighted
- Latency-based
- Geolocation
- Failover

---

## 6. Integration with AWS Services

Works seamlessly with:

- EC2
- ELB
- CloudFront
- S3
- API Gateway

---

# 🧩 Core Components

## 1. Domain Name

Human-readable name.

Example:

```text
example.com
```

---

## 2. Hosted Zone

Container for DNS records.

Stores:

- A records
- CNAME records
- MX records
- TXT records

---

## 3. DNS Record

Maps domain names to resources.

---

## 4. Name Servers (NS)

Servers responsible for DNS resolution.

---

# 🗂️ Hosted Zones

A hosted zone stores DNS records for a domain.

Two types:

| Type | Purpose |
|---|---|
| Public Hosted Zone | Internet-facing DNS |
| Private Hosted Zone | Internal VPC DNS |

---

# 🌍 Public Hosted Zone

Used for publicly accessible applications.

Example:

```text
www.example.com
```

Accessible from anywhere on the internet.

---

# 🔒 Private Hosted Zone

Used inside VPCs only.

Example:

```text
internal-db.company.local
```

Not accessible from the public internet.

---

# 📝 DNS Records

## 1. A Record

Maps domain to IPv4 address.

Example:

```text
example.com → 192.168.1.10
```

---

## 2. AAAA Record

Maps domain to IPv6 address.

---

## 3. CNAME Record

Maps one domain name to another domain name.

Example:

```text
app.example.com → example.com
```

---

## 4. MX Record

Used for email routing.

Example:

```text
mail.example.com
```

---

## 5. TXT Record

Stores text information.

Used for:

- Domain verification
- SPF records
- DKIM

---

## 6. NS Record

Specifies authoritative name servers.

---

## 7. SOA Record

Contains administrative DNS information.

---

# ⚖️ Routing Policies

Route 53 supports multiple routing methods.

---

## 1. Simple Routing

Single resource routing.

Example:

```text
example.com → One EC2 instance
```

---

## 2. Weighted Routing

Distributes traffic based on percentages.

Example:

| Server | Weight |
|---|---|
| Server A | 80 |
| Server B | 20 |

Used for:

- A/B testing
- Gradual deployments

---

## 3. Latency-Based Routing

Routes users to the lowest latency region.

Example:

- India users → Mumbai region
- US users → Virginia region

---

## 4. Failover Routing

Primary and secondary resources.

If primary fails:
- Traffic moves to secondary

Used for:
- Disaster recovery

---

## 5. Geolocation Routing

Routes traffic based on user location.

Example:

- India → Indian website
- Europe → EU website

---

## 6. Multi-Value Routing

Returns multiple healthy IPs.

Improves:
- Availability
- Load distribution

---

# ❤️ Health Checks

Route 53 can monitor:

- Websites
- APIs
- Servers

Checks:
- HTTP
- HTTPS
- TCP

If unhealthy:
- Traffic is redirected automatically

---

# 🌐 Domain Registration

Route 53 allows direct domain purchase.

Examples:

```text
example.com
mycompany.in
```

Features:

- Domain renewal
- WHOIS management
- DNS integration

---

# 🏷️ Alias Records

AWS-specific feature.

Used to map domains directly to AWS resources.

Supports:

- Load Balancer
- CloudFront
- S3 static website
- API Gateway

---

## Example

```text
example.com → ALB
```

Benefits:

- No extra DNS lookup
- Free queries for AWS resources

---

# ⏳ TTL (Time To Live)

TTL defines how long DNS responses are cached.

Example:

```text
TTL = 300 seconds
```

Lower TTL:
- Faster updates
- More DNS queries

Higher TTL:
- Better performance
- Slower updates

---

# 🔄 DNS Resolution Flow

```text
User Browser
   ↓
ISP Resolver
   ↓
Route 53 Hosted Zone
   ↓
DNS Record
   ↓
IP Address Returned
```

---

# ☁️ Route 53 with AWS Services

## 1. EC2

Maps domain to EC2 public IP.

---

## 2. Elastic Load Balancer

Maps domain to ALB/NLB.

---

## 3. CloudFront

Used for CDN routing.

---

## 4. S3 Static Website

Hosts static websites.

---

## 5. API Gateway

Custom domains for APIs.

---

# 🔐 Security

## IAM Policies

Control access to Route 53 resources.

---

## DNSSEC

Protects against DNS spoofing.

---

## Private Hosted Zones

Internal-only DNS resolution.

---

# 💰 Pricing

Charges based on:

- Hosted zones
- DNS queries
- Health checks
- Domain registration

---

# ✅ Advantages

- Highly available
- Globally distributed
- Supports advanced routing
- Deep AWS integration
- Managed DNS service
- Health checks included

---

# ❌ Disadvantages

- DNS propagation delays
- Complex routing configurations
- Can become costly at scale

---

# 💡 Best Practices

- Use Alias records for AWS resources
- Enable health checks
- Use low TTL during migrations
- Use failover routing for DR
- Separate public and private zones

---

# 🏢 Real-World Use Cases

## Global Applications

Latency-based routing improves performance.

---

## Disaster Recovery

Failover routing redirects traffic during outages.

---

## Blue/Green Deployment

Weighted routing gradually shifts traffic.

---

## Internal Company Applications

Private hosted zones for internal DNS.

---

# 🧠 Interview Questions

## What is Route 53?

A managed DNS and domain registration service provided by AWS.

---

## Why is it called Route 53?

Because DNS uses Port 53.

---

## What is a Hosted Zone?

A container for DNS records.

---

## Difference between Public and Private Hosted Zones?

| Public | Private |
|---|---|
| Internet accessible | Internal VPC only |
| Public websites | Internal applications |

---

## What is an Alias Record?

AWS-specific DNS record pointing directly to AWS resources.

---

## What is TTL?

Time DNS records remain cached.

---

## Difference between CNAME and Alias?

| CNAME | Alias |
|---|---|
| Standard DNS | AWS-specific |
| Cannot be root domain | Supports root domain |
| Extra DNS lookup | No extra lookup |

---

## What is Latency-Based Routing?

Routes users to the nearest AWS region based on latency.

---

# 📌 Important Exam Tips

Remember:

- Route 53 = Managed DNS service
- Supports domain registration
- Alias records are AWS-specific
- Supports advanced routing policies
- Health checks improve availability
- Private hosted zones work inside VPCs

---

# ✅ Conclusion

AWS Route 53 is a powerful and scalable DNS service that enables:

- Domain registration
- Traffic routing
- Health monitoring
- Global application delivery

It integrates deeply with AWS services and provides advanced routing capabilities for highly available and performant applications.

---
