# Rate Limiting in spring boot with Redisson

Rate limiting is a common technique used to control the amount of traffic that a service or application receives. By limiting the rate of incoming requests, you can prevent overloading of your servers and ensure that your application runs smoothly.

Redisson is a Java client library for Redis, a popular in-memory data structure store. Redisson provides several features for rate limiting, including support for both global and per-client rate limiting.

In this blog post, we'll walk through how to implement rate limiting in Spring Boot using Redisson.

## **Setting up Redisson**

Before we can start implementing rate limiting, we need to set up Redisson in our Spring Boot application. To do this, we'll add the Redisson dependency to our `pom.xml` file:

```xml
<dependency>
    <groupId>org.redisson</groupId>
    <artifactId>redisson</artifactId>
    <version>3.16.0</version>
</dependency>
```

Next, we'll create a Redisson client bean in our Spring Boot configuration. This bean will be used to connect to our Redis server and perform Redis operations:

```java
@Configuration
public class RedisConfig {

    @Value("${redis.host}")
    private String redisHost;

    @Value("${redis.port}")
    private int redisPort;

    @Bean(destroyMethod = "shutdown")
    public RedissonClient redissonClient() {
        Config config = new Config();
        config.useSingleServer().setAddress("redis://" + redisHost + ":" + redisPort);
        return Redisson.create(config);
    }
}
```

In this example, we're creating a Redisson client that connects to a Redis server running on [`localhost`](http://localhost) on the default Redis port of `6379`. We're using the `useSingleServer` method to configure the client to connect to a single Redis server.

## **Implementing rate limiting**

Now that we have Redisson set up, we can start implementing rate limiting. Redisson provides two types of rate limiting: global and per-client.

### **Global rate limiting**

Global rate limiting limits the rate of incoming requests for all clients. To implement global rate limiting in Redisson, we'll use a Redis `RRateLimiter` object. Here's an example:

```java
@RestController
public class ExampleController {

    private final RRateLimiter rateLimiter;

    public ExampleController(RedissonClient redissonClient) {
        this.rateLimiter = redissonClient.getRateLimiter("my-rate-limiter");
        // set rate limit to 3 requests per 1 Minute
        rateLimiter.trySetRate(RateType.OVERALL, 3, 1, RateIntervalUnit.MINUTES);
    }

    @GetMapping("/example")
    public String example() {
        // acquire a permit before processing the request
        boolean permitAcquired = rateLimiter.tryAcquire(1);
        if (!permitAcquired) {
            throw new TooManyRequestsException();
        }
        // process the request
        return "Example response";
    }
}
```

In this example, we're creating a `RRateLimiter` object with the name "my-rate-limiter". We're setting the rate limit to 10 requests per second using the `trySetRate` method. This sets the rate limit for all clients, since we're using the `OVERALL` rate type.

In the `example` method, we're using the `tryAcquire` method to acquire a permit before processing the request. If the rate limit has been exceeded, this method will return `false`, and we'll throw a `TooManyRequestsException`.

---

Let's check what is happening in the redis

```bash
127.0.0.1:6379> keys *
1) "my-rate-limiter"
127.0.0.1:6379> type my-rate-limiter
hash
127.0.0.1:6379> HGETALL my-rate-limiter
1) "rate"
2) "3"
3) "interval"
4) "60000"
5) "type"
6) "0"
127.0.0.1:6379>
```

As soon as you hit the first endpoint, two new keys are created value and permits, You can check their types and values with these commands.

```bash
keys *
1) "{my-rate-limiter}:value"
2) "{my-rate-limiter}:permits"
3) "my-rate-limiter"

127.0.0.1:6379> type {my-rate-limiter}:value
string
127.0.0.1:6379> type {my-rate-limiter}:permits
zset

127.0.0.1:6379> ZRANGE {my-rate-limiter}:permits 0 -1 WITHSCORES
 1) "mP'^\x01\x00\x00\x00"
 2) "1677392274348"
 3) "\xa7\xeeS\\\x01\x00\x00\x00"
 4) "1677392494709"
 5) "\x85G\x04\xde\x01\x00\x00\x00"
 6) "1677392495237"

get  {my-rate-limiter}:value
"0"
```

1677392274348 is the timestamp when the call was made, 26 February 2023 11:47:54.348 [GMT+05:30](https://www.epochconverter.com/timezones?q=1677392274348)

---

In a global rate limiting scenario, where you are limiting the number of requests across all clients or users, you may want to update the rate limiting properties dynamically, without having to restart the application. Redisson provides a way to achieve this by allowing you to update the configuration of the `RRateLimiter` object at runtime.

```java
 @GetMapping("/api/update-rate-limiter")
    public ResponseEntity<?> updateRateLimiter(@RequestParam int rate, @RequestParam int rateInterval) {
        rateLimiter.setRate(RateType.OVERALL, rate, rateInterval, RateIntervalUnit.SECONDS);
        return ResponseEntity.ok("Rate limiter updated" );
    }
```

trySetRate - Initializes RateLimiter's state and stores config to Redis server.

setRate - Updates RateLimiter's state and stores config to Redis server.

trySetRate uses [HSETNX](https://redis.io/commands/hsetnx/) redis command  
Sets `field` in the hash stored at `key` to `value`, only if `field` does not yet exist.  
If `field` already exists, this operation has no effect.  
  
setRate uses [`HSET`](https://redis.io/commands/hset/),

This command overwrites the values of specified fields that exist in the hash. If `key` doesn't exist, a new key holding a hash is created.

```yaml
http://localhost:8080/api/update-rate-limiter?rate=10&rateInterval=1
```

```bash
127.0.0.1:6379> HGETALL my-rate-limiter
1) "rate"
2) "10"
3) "interval"
4) "1000"
5) "type"
6) "0"
```

You can check all the redis commands that redission is enabling redis logs, For steps check this: [https://til.hashnode.dev/enable-redis-logs](https://til.hashnode.dev/enable-redis-logs)

### **Per-client rate limiting**

Per-client rate limiting limits the rate of incoming requests for each client individually. To implement per-client rate limiting in Redisson, we'll use a Redis `RPerClientRateLimiter` object. Here's an example:

```java
@RestController
public class ExampleController {
    private final RRateLimiter rateLimiter;

    public ExampleController(RedissonClient redissonClient) {
        this.rateLimiter = redissonClient.getRateLimiter("my-rate-limiter");
        // set rate limit to 10 requests per second per client
        rateLimiter.trySetRate(RateType.PER_CLIENT, 10, 10, RateIntervalUnit.MINUTES);
    }

    @GetMapping("/example")
    public String example() throws TooManyRequestsException {
        // acquire a permit before processing the request
        boolean permitAcquired = rateLimiter.tryAcquire(1);
        if (!permitAcquired) {
            throw new TooManyRequestsException();
        }
        // process the request
        return "Example response left " + rateLimiter.availablePermits();
    }

    @GetMapping("/api/update-rate-limiter")
    public ResponseEntity<?> updateRateLimiter(@RequestParam int rate, @RequestParam int rateInterval) {
        rateLimiter.setRate(RateType.PER_CLIENT, rate, rateInterval, RateIntervalUnit.SECONDS);
        return ResponseEntity.ok("Rate limiter update" );
    }
}
```

In this example, we're creating a `RPerClientRateLimiter` object using the `getPerClientRateLimiter` method. We're setting the rate limit to 10 requests per minute per client using the `trySetRate` method and the `PER_CLIENT` rate type. In the `example` method, we're using the `tryAcquire` method to acquire a permit before processing the request. We're passing the client's IP address as the key to the `tryAcquire` method, so the rate limit will be applied per client.

---

```bash
127.0.0.1:6379> HGETALL my-rate-limiter
1) "rate"
2) "10"
3) "interval"
4) "1000"
5) "type"
6) "1"
```

If you run two instances of the same application, and use the common rate limiter config

```bash
keys *
1) "{my-rate-limiter}:permits:0b68ca6e-1b46-49cd-9279-6d0b1313a09d"
2) "{my-rate-limiter}:permits:aec1fd29-75af-42b9-b2dc-98f239d86d38"
3) "{my-rate-limiter}:value:aec1fd29-75af-42b9-b2dc-98f239d86d38"
4) "{my-rate-limiter}:value:0b68ca6e-1b46-49cd-9279-6d0b1313a09d"
5) "my-rate-limiter"
```

---

Redisson's per-client rate limiting feature can be useful in a scenario where you have multiple instances of the same application running in parallel, and you want to add a smaller rate limit per instance. This can be useful in a distributed system where multiple instances of an application are processing requests concurrently and you want to limit the number of requests that each instance can handle.

### Conclusion

In this blog post,we learned how to implement rate limiting in Spring Boot using Redisson. We've covered both global and per-client rate limiting. By using rate limiting, you can prevent overloading of your servers and ensure that your application runs smoothly, even during periods of high traffic. Redisson provides a simple and powerful way to implement rate limiting in your Spring Boot applications.

If you want to implement a basic rate limiter in redis check this

[Basic Rate Limiting](https://redis.com/redis-best-practices/basic-rate-limiting/)

[Redisson Rate limiter](https://redisson.org/glossary/rate-limiter.html)

Check the code for this example: [https://github.com/nkalra0123/rate-limiting](https://github.com/nkalra0123/rate-limiting)

If you liked this blog, [**you can follow me on twitter**](https://twitter.com/nkalra0123), and learn something new with me.