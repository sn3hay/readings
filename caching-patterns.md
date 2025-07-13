## Caching Patterns

### Cache Aside (Lazy-Loading)

* Reads:
  * The cache is **queried** first. If it has the data, it is a cache hit and the data is returned to the user.
  * If the cache does not have the data, it is a cache miss and the database is queried for the data. The cache is also updated with the data received from the database.
* Writes:
  * Writes are performed directly on the database, the cache is invalidated or bypassed.
  * Since cache is not updated after a write, there are two options:
    * Do nothing - stale data in cache
    * Invalidate cache - remove the outdated key
* This is cost effective
* Data is loaded into the cache only after a cache miss
* Better for frequent writes

### Read Through Cache
* Applications will read only from the cache; Cache will know how to fetch data from **database** in case of cache miss
* Reads:
  * Good for reads, since the responsibility of fetching data in case of a cache miss is on the cache.
  * Provides abstraction on where the data actually came from
* Writes:
  * The **application** is burdened with writing to both, the cache as well **as** the database.
  * It can either write to the database and
    * Invalidate cache OR
    * Update cache
* Writes are not automatically updated in the cache
* Centralized caching (good when users have to view the same data, invalidating or updating is also limited to just one place)

### Write Through Cache
* Data is proactively updated after the primary database has been updated.
* Reads:
  * Likelihood of data to be found in cache is higher.
  * This strategy is **good for frequent reads**
* Writes:
  * Both cache and database are to be updated in the same flow. So writes go to cache and writes also go to DB
  * Higher write latency because, since we are writing to both cache and database, **we have to use protocols like Two Phase Commit to ensure that the data in cache and database is the same updated data**.
* Infrequent data will now also reside on cache, which is a disadvantage.

### Write Back/Behind

* Data in database is updated **asynchronously** (by the cache) after the data is written to the cache. Could be in batches.
* Reads:
  * Good for high reads and writes
* Writes:
  * If writes are delayed to the DB, it can lead to data inconsistency
* Data might be eventually consistent.


**References:**
[https://docs.aws.amazon.com/whitepapers/latest/database-caching-strategies-using-redis/caching-patterns.html](https://docs.aws.amazon.com/whitepapers/latest/database-caching-strategies-using-redis/caching-patterns.html)
