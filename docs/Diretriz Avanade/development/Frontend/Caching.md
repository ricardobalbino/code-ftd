# Caching

UX has a tremendous effect on the overall acceptance of the solution and the end user adoption rate. Utilizing a fast, responsive and reliable UI the life a project is more relaxing that the other way around.

Unfortunately the Web API of Dataverse does not support Browser based resource caching. Calling the API with a request including a `HTTP cache header` Dataverse always returns with `no-cache` which forces the Browser not to apply no response caching resulting in calls being fired against the API over and over again.

## Why should I cache?
As described, caching increases the responsiveness of the UI and in addition it reduces the load on your Dataverse environment. The latter even helps to _mitigate_ wrong capacity/sizing estimations.

## What should be cached?
There are two types of caching which you could apply.

### Stable data
This should be the most obvious one. Every dataset or combination of records which tend to be/are stable should be cached. For instance something like
- User team assignments
- User/Team Security Roles
- Configuration data
- etc.

### Mitigate request duplication
Depending on how you solved the structure of the frontend code or if you have a big and complex project you may end up firing the same query/request against the Web API multiple times during an onLoad of any Model Driven Form (Detail/Home/Webresource).

You could utilize a cache which exists as long as a usual page load takes in the system. For instance cache the URI response for 3 seconds. If by any reason a page load exceeds the 3 seconds the logic will still work just fine, it just fires another call against the Web API. If it is inside the timeframe of 3 seconds it will serve the same request once more.

## How does caching work
Caching is utilized by providing an additional layer above the odata execution layer used in [Commands](../Backend/Commands.md) and the [OData Query Builder](OData-Query-Builder.md).

Every odata request is just a HTTP representation of what needs to be done/executed. This HTTP request itself will be used to generate the key which will be used to determine if a request with the "payload" was alredy being executed. The resulting key will be generated via `js-sha256`'s hashing function.

This classes which do the trick are called `DefaultCache` & `CacheItem`. The `CacheItem` is the combination
- The hashed key
- The expiration timestamp [`Date().getTime() + X`]
- Cached response

You can use the cache outside of the `Commands` or the `OData Query Builder` by importing the cache instance like this:

```ts
import { CacheItem, CacheInstance } from "./DefaultCache";
```

The `DefaultCache` will be provided via a Singleton-Pattern to enforce the usage of only one instance of it. Using the `CacheInstance` the `DefaultCache` provides different "public" methods which you can use:

### Adding new cache items

```ts
    const cacheItem = new CacheItem(request, expireOn, response);
    CacheInstance.addToCache(cacheItem);
```

## Cache lifetime

We're utilizing the browser's native session storage implementation. Because we're using the session storage closing the browser/tab will invalidate the cache thus we don't need to cater with clearing the browser cache.
