# AWS CloudFront — Caching and Policies

## Cache Hit

If an object exists in the CloudFront cache:

```text
User
 |
 v
CloudFront Edge
 |
 | Cache Hit
 v
Cached Object
```

The origin is not contacted.

## Cache Miss

If the object is not cached:

```text
User
 |
 v
CloudFront Edge
 |
 | Cache Miss
 v
Origin
 |
 v
CloudFront Edge
 |
 v
User
```

CloudFront retrieves the object and can cache it.

## TTL

**TTL (Time To Live)** determines how long an object can remain cached.

Example:

```text
Cache-Control: max-age=3600
```

3600 seconds = 1 hour.

Important controls:

- Minimum TTL
- Maximum TTL
- Default TTL
- Cache-Control
- Expires

## Cache Policy

A **Cache Policy** controls the cache key and therefore what makes two requests different cached objects.

It can consider:

- Query strings
- Headers
- Cookies

Example:

```text
/image.jpg?size=small
/image.jpg?size=large
```

If the query string is part of the cache key, these can be separate cache objects.

## Origin Request Policy

An **Origin Request Policy** controls what CloudFront forwards to the origin.

```text
Cache Policy
    |
    +---- Controls caching / cache key

Origin Request Policy
    |
    +---- Controls information sent to origin
```

## Cache Invalidation

When new content is deployed, CloudFront may still have the old object cached.

You can invalidate:

```text
/*
```

or a specific object:

```text
/index.html
```

CLI:

```bash
aws cloudfront create-invalidation   --distribution-id DISTRIBUTION_ID   --paths "/*"
```

## Versioned Assets

Instead of frequently invalidating static assets:

```text
app.v1.js
app.v2.js
app.v3.js
```

Changing the filename creates a new cache key.

## Best Practices

- Cache static content for longer periods.
- Avoid incorrectly caching dynamic content.
- Use versioned assets.
- Configure cache policies carefully.
- Monitor cache hit ratio.
- Use invalidation when necessary.
