# Storage considerions
- While considering STORAGE, I assumed if Urls stay in system for lifetime, do I need a cleanup at all ? 
- ```Lifetime” does NOT mean “forever in hot storage with no cleanup```
- Look for opportunities of Storage Lifecycles
- Cleanups also mean audit cleanup, cache evictions etc .
- We can store urls in hot or cold storage based on its retrieval frequencies.

# 301 vs 302 (also why not 307)
301 Redirection status code essentially denotes the permanent movement of the URL. It prompts the browser to cache the redirect and never visit the URL shortener service that we built **;** instead, it tells the browser to always go directly to the long URL. With this approach, we risk losing control over our URL service. It can expose us to risks of URL abuse, and we would not be able to apply rate limits **,** etc. We won't even be able to track analytical activities for these URLs.

302, on the other hand, never caches the redirect information, causing browsers to always visit the shortener service. There is also 307, but that would not essentially make a difference in our case since the URLs we use are accessed via GET requests. 307 specializes in retaining the HTTP method that was sent **,** while 302 tends to convert non-GET HTTP methods to GET.
# Sharding
- Crucial for scalability. This falls under **horizontail scaling**. This is importing to avoid SPOF (Single point of failure)
- Each shard holds one subset of data. In our scenario, around 50 TB of data can be divided into 5 shards of 10TB each. This would help in distributing load across multiple servers.
- There are different Sharding techniques. For url shortener, we can leverage hash based sharding . Here each shard would contain urls based on which hash the short url may fall. Example:

|Shard 1| Sahrd 2|Shard 3|Shard 4| Shard 5|
|-------|--------|-------|-------|--------|
|a-e    |f-j     |k-o    |p-t    |u-z     |
|10 TB  |10 TB   |10 TB  |10 TB  |10 TB   |

# Group Discussion Learning 

#### Adarsh
- For large url suit, Hashing would cause collision. hence going with base 64 encoding. 
- the entire URL is mapped to a number, and that number is encoded using base 64, promoting uniqueness, and availability.

#### Atharv
- Focus was on case insensitivity, hence went with Base36 encoding.
- Used Postgres because it is thread safe an handles concurrency.
    - May also have one risk of finding the sequence in above sequential encoding, and bot can overwhelm the service.

#### Upamanyu
- Bloom Filters
- Encoded Sequence, chunking of the entire url, and encoding each sequence of the url chunk.