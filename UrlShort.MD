# Problem Statement
Design a system that generates unique short URLs for given long URLs, ensuring efficient storage and quick retrieval. Focus on the encoding algorithm, database design, and how to handle high read-write ratios and potential collisions.
[Website Link here](https://bugfree.ai/practice/system-design/url-shortener)

# Requirements
### Functional Requirements
- Create shortest possible URLs for given URL
- Redirectto original URL via short url
- No short url expiry
- Metric collection { _nice to have_ }
### Non Funcitional Requirement
- Up and Running system , at peak hours.

# Assumptions
- Short urls would not expire and would be active for lifetime.
- Assuming **10M** URLs generated per day
    - 10M/24/3600 = ~1160 URL creations per second
    - Assuming Higher traffic , let us consider for 1500 url creations per second
    - Taking read write ratio as 100:1 , 150k url reads per second
- Assuming **100 bytes** per url entry
    - 1500 urls per second for an application that would run for 10 years would typically have ≈ 473 billion URLs
    - ≈ 47.3 TB of raw storage. 
    - Must also consider indexes and cache storage

# API Design
- Going with the industry standard REST API suire

### Create a shortened URL
> **Note**
> - `Idempotency-Key` is optional and prevents duplicate creation during retries.
> - Authentication is optional depending on product requirements.

---

POST : /createUrl
#### Request Body
```json
{
  "long_url": "https://example.com/some/very/long/url",
  "custom_alias": "my-link",
  "expiry": "2026-01-01T00:00:00Z"
}
```
#### Response
status code: 201
``` json
{
  "short_url": "https://sho.rt/abc123",
  "short_code": "abc123",
  "created_at": "2026-01-17T10:00:00Z"
}
```

### Retrieve the long url

#### Request
``` json
GET /{short_code}
```
#### Response
``` json
GET /{short_code}
```

# Trade Off
Q. Do we allow creating multiple short URLs for one long url, to maintain idempotency ?
A. 