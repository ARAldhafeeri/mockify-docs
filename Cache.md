## Redis cache and configuration

Cache page in [mockify.io](http://mockify.io) allow you to create, manage cache, get,set keys , view your cached data. There are three new endpoints  :

- Allow users mock cache and access it  within edge function
- Cache page lists k,v with create, delete, update abilities.
- POST  v1/cache/:projectName endpoint : Mock  API endpoint to set  k/v within cache
- GET v1/cache/:projectName  endpoint : Mock API endpoint to get  a k/v within cache
- DELETE v1/cache/:projectName  endpoint: Mock API endpoint to delete  a k/v within cache
- GET v1/cache/all /:projectName  return all cache data for a specific project.
- Multi-tenant cache database.

# multi tenancy in Redis

- Access to cache is  restricted per API key.
- APIKey is linked to projectName, projectName is there with every request url to the cache
- User set keys as plain strings e.g.

```json
SET SomeKey Value
```

Mockify implicitly hide the following detail from the user, ito achieve multi-tenancy , it will set the key with the following prefix  projectName:someKey

```json
SET ProjectName:SomeKey Value
```

Then getting all the keys  for the user is using the following wild card

```json
GET projectname:*
```

## Using cache within edge function

```json
CacheGet(k)
CacheSet(k,v)
```

The following functions are added to edge function context,  user can get, set keys using the added context. User can provide plain string,mockify will add projectName:key for each get, set request.

### Example covering mock cache requirement TTL

**Time-to-Live (TTL):** You can set an expiration time for each key in Redis using the TTL feature. After a specified period, the key will automatically be deleted. This is useful for automatically refreshing the cache periodically.

In [mockify.io](http://mockify.io) you can cover this requirement using newly added edge context 

```json
CacheGet
CacheSet
```

Example : 

- We defined function name EdgeTest
- For resource default

```json
await CacheSet('test:test', 'test', 1000);
data = await CacheGet('test:test');
```

Request :

```json
http://localhost:5000/v1/default/edge/edgeTest
```
Response: 

```json
{"status":true,"message":"fetching data was successful","data":"test"}
```