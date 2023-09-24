---
title: "Transactions in Redis"
datePublished: Sun Sep 24 2023 06:32:41 GMT+0000 (Coordinated Universal Time)
cuid: clmx31qus000209l80r1jblab
slug: transactions-in-redis
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/xkArbdUcUeE/upload/184f7894d2e40b6fccc0e5f9b334aae3.jpeg
tags: redis, transactions, redis-cluster

---

Redis is a popular in-memory data store that supports a wide range of data structures and operations.

**Transactions in Redis are not like transactions in, say a SQL database.**

Transaction allows a group of commands to be executed as a single atomic operation.

This means that either all of the commands in the transaction are executed or none of them are. Transactions in Redis are implemented using the MULTI and EXEC commands.

**A transaction in redis consists of a block of commands placed between** `MULTI` **and** `EXEC` **(or** `DISCARD` **for rollback).**

The **MULTI** command starts a new transaction, and all subsequent commands are queued up to be executed as part of the transaction. The **EXEC** command then executes all of the commands in the transaction.

Once a `MULTI` has been encountered, the commands on that connection *are not executed* - they are queued (and the caller gets the reply `QUEUED` to each). When an `EXEC` is encountered, they are all applied in a single unit (i.e. without other connections getting time between operations). If a `DISCARD` is seen instead of a `EXEC`, everything is thrown away.

```bash
MULTI
OK
127.0.0.1:7001(TX)> SET key1 "value1001"
QUEUED
127.0.0.1:7001(TX)> get key1
QUEUED
127.0.0.1:7001(TX)> EXEC
1) OK
2) "value1001"
```

```java
Jedis jedis = new Jedis("localhost");
Transaction t = jedis.multi();
t.set("key1", "value1");
t.set("key2", "value2");
t.exec();
```

**Because the commands inside the transaction are queued, you can’t make decisions *inside* the transaction**

Redis Transactions make two important guarantees:

* All the commands in a transaction are serialized and executed sequentially. A request sent by another client will never be served **in the middle** of the execution of a Redis Transaction. This guarantees that the commands are executed as a single isolated operation.
    
* The [`EXEC`](https://redis.io/commands/exec) command triggers the execution of all the commands in the transaction, so if a client loses the connection to the server in the context of a transaction before calling the [`EXEC`](https://redis.io/commands/exec) command none of the operations are performed, instead if the [`EXEC`](https://redis.io/commands/exec) command is called, all the operations are performed.
    

## **Errors inside a transaction**

During a transaction it is possible to encounter two kind of command errors:

* A command may fail to be queued, so there may be an error before [`EXEC`](https://redis.io/commands/exec) is called. For instance the command may be syntactically wrong (wrong number of arguments, wrong command name, ...), or there may be some critical condition like an out of memory condition (if the server is configured to have a memory limit using the `maxmemory` directive).
    
* A command may fail *after* [`EXEC`](https://redis.io/commands/exec) is called, for instance since we performed an operation against a key with the wrong value (like calling a list operation against a string value).
    

server will detect an error during the accumulation of commands. It will then refuse to execute the transaction returning an error during [`EXEC`](https://redis.io/commands/exec), discarding the transaction.

```bash
MULTI
OK
127.0.0.1:7001(TX)> get key1 asd
(error) ERR wrong number of arguments for 'get' command
127.0.0.1:7001(TX)> SET key1 "value1001"
QUEUED
127.0.0.1:7001(TX)> EXEC
(error) EXECABORT Transaction discarded because of previous errors.
```

This is the example for the first scenario.

Errors happening *after* [`EXEC`](https://redis.io/commands/exec) instead are not handled in a special way: all the other commands will be executed even if some command fails during the transaction.

```bash
SET key1 "value1001"
QUEUED
127.0.0.1:7001(TX)> SET key1 "value1001" abc
QUEUED
127.0.0.1:7001(TX)> EXEC
1) OK
2) (error) ERR syntax error
```

In this case, first command is executed and second failed.

So transactions are not like SQL databases, where it is rolled back in case of some error.

> **Redis does not support rollbacks of transactions since supporting rollbacks would have a significant impact on the simplicity and performance of Redis.**

This above quote is from Redis documentation.

It is like saying, this is beyond the scope of this book. In my view, these should have been named differently (maybe sequential operation) rather than transactions.

Now database support optimistic locking and pessimistic locking in transactions, so Redis supports **Optimistic locking using check-and-set**

[`WATCH`](https://redis.io/commands/watch) is used to provide a check-and-set (CAS) behavior to Redis transactions.

`WATCH`ed keys are monitored in order to detect changes against them. If at least one watched key is modified before the `EXEC` command, the whole transaction aborts, and `EXEC` returns a **Null reply** to notify that the transaction failed.

```bash
WATCH mykey
val = GET mykey
val = val + 1
MULTI
SET mykey $val
EXEC
```

Using the above code, if there are race conditions and another client modifies the result of `val` in the time between our call to [`WATCH`](https://redis.io/commands/watch) and our call to [`EXEC`](https://redis.io/commands/exec), the transaction will fail.

We just have to repeat the operation hoping this time we'll not get a new race.

With this approach, you can make decisions inside a transaction(locked with optimistic locks)

### Transactions in Redis cluster

Transactions in Redis cluster are limited to a single hash slot, which means that all keys involved in a transaction must belong to the same hash slot. Additionally, WATCH command can only be used to monitor keys in the same hash slot as the key being watched. If you need to perform transactions across multiple hash slots, you can use Lua scripting instead.

To ensure the same hash slot, we can use the concept of [Hashtag](https://redis.io/docs/reference/cluster-spec/#hash-tags)

If you liked this blog, [**you can follow me on twitter**](https://twitter.com/nkalra0123), and learn something new with me.