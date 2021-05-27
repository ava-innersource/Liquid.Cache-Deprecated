Liquid Application Framework - Cache
====================================

This repository is part of the [Liquid Application Framework](https://github.com/Avanade/Liquid-Application-Framework), a modern Dotnet Core Application Framework for building cloud native microservices.

The main repository contains the examples and documentation on how to use Liquid.

Liquid.Cache
------------
[![Quality Gate Status](https://sonarcloud.io/api/project_badges/measure?project=Avanade_Liquid.Cache&metric=alert_status)](https://sonarcloud.io/dashboard?id=Avanade_Liquid.Cache) 

This package contains the caching subsystem of Liquid, along with several caching implementation. In order to use it, add the main package (Liquid.Cache) to your project, along with the specific implementation that you will need. Liquid takes care of the serialization

Available implementations:
|Available Cartridges|Badges|| 
|:--|--|-------|
|[Liquid.Cache.Redis](https://github.com/Avanade/Liquid.Cache/tree/main/src/Liquid.Cache.Redis)|[![Quality Gate Status](https://sonarcloud.io/api/project_badges/measure?project=Avanade_Liquid.Cache.Redis&metric=alert_status)](https://sonarcloud.io/dashboard?id=Avanade_Liquid.Cache.Redis)| Cache implementation using Redis as a backing service.|
|[Liquid.Cache.Ignite](https://github.com/Avanade/Liquid.Cache/tree/main/src/Liquid.Cache.Ignite)|[![Quality Gate Status](https://sonarcloud.io/api/project_badges/measure?project=Avanade_Liquid.Cache.Ignite&metric=alert_status)](https://sonarcloud.io/dashboard?id=Avanade_Liquid.Cache.Ignite)| Cache implementation using Apache Ignite as a backing service.|
|[Liquid.Cache.Memory](https://github.com/Avanade/Liquid.Cache/tree/main/src/Liquid.Cache.Memory)|[![Quality Gate Status](https://sonarcloud.io/api/project_badges/measure?project=Avanade_Liquid.Cache.Memory&metric=alert_status)](https://sonarcloud.io/dashboard?id=Avanade_Liquid.Cache.Memory)| Local in memory cache. Primarily to be used for mocking / testing.|
[Liquid.Cache.NCache](https://github.com/Avanade/Liquid.Cache/tree/main/src/Liquid.Cache.NCache)|[![Quality Gate Status](https://sonarcloud.io/api/project_badges/measure?project=Avanade_Liquid.Cache.NCache&metric=alert_status)](https://sonarcloud.io/dashboard?id=Avanade_Liquid.Cache.NCache)| Caching implementation based on NCache distributed caching.|

Getting Started
-----

This is a sample usage with the main methods of the cache.
```C#
using Liquid.Cache;
```

```C#
namespace Liquid.Samples.Cache
{
    public class SampleCache 
    {

        // Obtain your copy of the configured cache via Dependency Injection
        private readonly ILightCache _cache;

        public SampleCache(ILightCache cache) 
        {
            _cache = cache;
        }

        public async CacheUsage() {
            // obtains a value from the cache
            var myvalue = await _cache.RetrieveAsync<string>("mycacheKey");
            var myvalue2 = await _cache.RetrieveAsync<string>("mycacheKey2") ?? "defaultValue";
            // add a value to the cache, with 1 minute expiration time
            await _cache.AddAsync("mycacheKey", "my value", TimeSpan.FromMinutes(1));
            // checks for  existence of key
            var exists = await _cache.ExistsAsync("mycachekey");
        }
    }
}
```
Dependency Injection (sample using Redis Cartridge)
```C#
using Liquid.Cache.Memory;
```
```C#
services.AddDefaultTelemetry();
services.AddDefaultContext();
services.AddLightRedisCache();
```
appsettings.json
```Json
{
  "liquid": {
    "cache": {
      "redis": {
        "connectionString": "localhost:6379,ssl=false,allowAdmin=true, asyncTimeout=10000"
      }
    }
  }
}
```