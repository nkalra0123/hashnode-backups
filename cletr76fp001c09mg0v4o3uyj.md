---
title: "Rate Limiting in spring boot with Redisson - Part 2"
datePublished: Sat Mar 04 2023 09:20:02 GMT+0000 (Coordinated Universal Time)
cuid: cletr76fp001c09mg0v4o3uyj
slug: rate-limiting-in-spring-boot-with-redisson-part-2
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/53B17GiIhTA/upload/9008281b1d3afed5c601b0c442f00978.jpeg
tags: redis, springboot, ratelimit, reddison

---

> You can check all the redis commands that redission is running by enabling redis logs, For steps check this: [**https://til.hashnode.dev/enable-redis-logs**](https://til.hashnode.dev/enable-redis-logs)

Let's see the logs

```bash
"EVAL" 
"redis.call('hsetnx', KEYS[1], 'rate', ARGV[1]);
redis.call('hsetnx', KEYS[1], 'interval', ARGV[2]);return redis.call('hsetnx', KEYS[1], 'type', ARGV[3]);" "1" "my-rate-limiter" "3" "60000" "0"
```

The Redis command is "EVAL" command, which is used to evaluate Lua scripts in the Redis server. The general syntax of the EVAL command is as follows:

```bash
EVAL script numkeys key [key ...] arg [arg ...]
```

The arguments in this example command are as follows:

* `script`: The Lua script to be executed.
    
* `numkeys`: The number of keys that the script will access.
    
* `key [key ...]`: The key(s) that the script will access.
    
* `arg [arg ...]`: The argument(s) that the script will use.
    

The Lua script in this command is:

```bash
1677915630.591309 [0 lua] "hsetnx" "my-rate-limiter" "rate" "3"
1677915630.591341 [0 lua] "hsetnx" "my-rate-limiter" "interval" "60000"
1677915630.591349 [0 lua] "hsetnx" "my-rate-limiter" "type" "0"
```

This script uses the Redis `call` function to execute three Redis commands in sequence:

1. `hsetnx` - This command sets the value of a hash field only if the field does not already exist. In this case, it sets the `rate` field of the hash at `KEYS[1]` to the value of `ARGV[1]`.
    
2. `hsetnx` - This command sets the value of a hash field only if the field does not already exist. In this case, it sets the `interval` field of the hash at `KEYS[1]` to the value of `ARGV[2]`.
    
3. `hsetnx` - This command sets the value of a hash field only if the field does not already exist. In this case, it sets the `type` field of the hash at `KEYS[1]` to the value of `ARGV[3]`.
    

The arguments to the command are as follows:

* `1`: `numkeys` - The number of keys that the script will access. In this case, the script will access one key.
    
* `my-rate-limiter`: `key` - The key that the script will access. In this case, it is the key `my-rate-limiter`.
    
* `3`: `ARGV[1]` - The value that will be set as the `rate` field in the hash at `KEYS[1]`.
    
* `60000`: `ARGV[2]` - The value that will be set as the `interval` field in the hash at `KEYS[1]`.
    
* `0`: `ARGV[3]` - The value that will be set as the `type` field in the hash at `KEYS[1]`.
    

All these commands are executed when this code is executed

```bash
public ExampleController(RedissonClient redissonClient) {
    this.rateLimiter = redissonClient.getRateLimiter("my-rate-limiter");
    // set rate limit to 3 requests per 1 Minute
    rateLimiter.trySetRate(RateType.OVERALL, 3, 1, RateIntervalUnit.MINUTES);
}
```

Now when you hit the API, these commands are executed

```java
boolean permitAcquired = rateLimiter.tryAcquire(1);
```

```bash
"EVAL" "local rate = redis.call('hget', KEYS[1], 'rate');

local interval = redis.call('hget', KEYS[1], 'interval');

local type = redis.call('hget', KEYS[1], 'type');

assert(rate ~= false and interval ~= false and type ~= false, 'RateLimiter is not initialized')

// KEYS[1] = "my-rate-limiter"
```

With These commands we get the rate, interval, and type (RateType.OVERALL = 0 or RateType.PER\_CLIENT = 1), and if these values are not set, we throw an error.

```bash
local valueName = KEYS[2];
local permitsName = KEYS[4];
if type == '1' then valueName = KEYS[3];
permitsName = KEYS[5];
end;
assert(tonumber(rate) >= tonumber(ARGV[1]), 'Requested permits amount could not exceed defined rate'); 

// ARGV[1] = "1" -- requested number of permits 
// KEYS[2] = "{my-rate-limiter}:value"
// KEYS[3] = "{my-rate-limiter}:value:62556984-2963-4021-a640-1f556ae9948d" 
// KEYS[4] = "{my-rate-limiter}:permits"
// KEYS[5] = "{my-rate-limiter}:permits:62556984-2963-4021-a640-1f556ae9948d"
```

with these commands we set the variables valueName, permitsName depending on the type of rate limiter we created.

and we throw an error if the requested permits are greater than the defined rate.

```bash
rate = 3 
interval = 60000
valueName = "{my-rate-limiter}:value"
permitsName = "{my-rate-limiter}:permits"
```

```bash
local currentValue = redis.call('get', valueName); 
// As valueName is not set initally, if condition in next code block is not executed, and else portion runs.

redis.call('set', valueName, rate); 
redis.call('zadd', permitsName, ARGV[2], struct.pack('fI', ARGV[3], ARGV[1])); 
redis.call('decrby', valueName, ARGV[1]); 
return nil; 

// set the value to 3, save the timestamp, and reduce the value by 1
```

```bash
all this code is executed currentValue is set, and has some value

if currentValue ~= false 
    then local expiredValues = redis.call('zrangebyscore',         permitsName, 0, tonumber(ARGV[2]) - interval); 

// ARGV[2] = "1677916085668" == Saturday, 4 March 2023 13:18:05.668
// "tonumber(1677916085668) - 60000"  = 1677916025668
// 1677916025668 == Saturday, 4 March 2023 13:17:05.668

Find expiredValues in last 1 min 

local released = 0; 
for i, v in ipairs(expiredValues) 
do local random, permits = struct.unpack('fI', v);
released = released + permits;
end; 
```

Find all the values from "{my-rate-limiter}:permits" that has to be removed, as these values are before the 1 min window, that we defined

```bash
if released > 0 
then 
redis.call('zremrangebyscore', permitsName, 0, tonumber(ARGV[2]) - interval); 

currentValue = tonumber(currentValue) + released; 

redis.call('set', valueName, currentValue);
end;
```

Now remove these values, found in the previous step.

```bash
currentValue = tonumber(currentValue) + released; 
```

With this we find, the number of new permits that we have, and set that value in "{my-rate-limiter}:value"

```bash
if tonumber(currentValue) < tonumber(ARGV[1]) 
    then local nearest = redis.call('zrangebyscore', permitsName, '(' .. (tonumber(ARGV[2]) - interval), '+inf', 'withscores', 'limit', 0, 1); return tonumber(nearest[2]) - (tonumber(ARGV[2]) - interval);

else 
redis.call('zadd', permitsName, ARGV[2], struct.pack('fI', ARGV[3], ARGV[1])); 
redis.call('decrby', valueName, ARGV[1]); return nil; 
end; 
```

If currentValue &gt; requested permit, we save the timestamp and reduced the value by 1

The expression `tonumber(nearest[2]) - (tonumber(ARGV[2]) - interval)` calculates the time until the next permit becomes available. It subtracts the current time (`tonumber(ARGV[2])`) from the expiry time of the permit (`tonumber(nearest[2])`) and then adds the interval duration (`interval`). This gives the time until the expiry of the current time window, plus the time until the next permit becomes available.

You can check the formatted code in Redisson github repo: [https://github.com/redisson/redisson/blob/c432023f2735421f1e1998f94ff10e9012bd5f71/redisson/src/main/java/org/redisson/RedissonRateLimiter.java#L178](https://github.com/redisson/redisson/blob/c432023f2735421f1e1998f94ff10e9012bd5f71/redisson/src/main/java/org/redisson/RedissonRateLimiter.java#L178)