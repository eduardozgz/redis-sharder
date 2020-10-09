# redis-sharder

Redis sharder is a great solution for sharding with [Eris](https://github.com/abalabahaha/eris). Managing shards for your large discord bot can be a major headache but with Redis Sharder all you need to do is code your bot and not worry about the issues of spawning and managing tons of shards. Redis Sharder is almost a drop in replacement and uses Eris's internal sharding. Redis sharder is **not** a clustering solution. You will need to cluster your bot yourself via some means or a process manager.

# 2.0.0 TODO
- x16, x64, etc large bot sharding
  - Wont be done until we know exactly how it works
- ~~Shared REST ratelimits~~
  - ~~This is something I want... but will be super complicated (and slow?)~~
- Workers
- Pub sub
- Statistics
- Tests/Examples
- Documentation

### Clustering Solutions 
- Kubernetes
- Docker Swarm
- PM2 Clustering
- NodeJS Cluster Module

### Users of Redis Sharder:
*As of 10/7/2020*. 
| Name | Guild Count | Shard Count | 
| - | -| - |
| [Arcane](https://arcanebot.xyz) | 203,000 | 224 |
| [Server Captcha Bot](https://top.gg/bot/captcha) | 38,000 | 50 |

### How to determine shardsPerCluster & cluster count
We recommend running 16 to 32 shards a cluster. In [Arcane's](https://arcanebot.xyz) case we have been able to push 32 shards stabily.
```
total_shards / shards_per_cluster -> round down = cluster count
```
Redis sharder will allocate shards_per_cluster to all processes except for the last process. The last process will be allocated any extras ontop of the shards_per_cluster. 

### Help
Join our [discord](https://discord.gg/JBwVquz). We expect you to know how to code. We will not spoonfeed you. Definitely look through the src of redis sharder so you can view some of the utilities it provides.

# Installation

- Add redis-sharder to your project. `npm install @arcanebot/redis-sharder`
- Install and secure [Redis](https://redis.io/) on your machine. Redis Sharder requires redis to be secured with a password to use any eval functions because Redis is used for [Pub Sub](https://en.wikipedia.org/wiki/Publish%E2%80%93subscribe_pattern). **PLEASE USE AUTHORIZATION!**

# Examples
You can view our quick start example [here](https://github.com/arcanebot/redis-sharder/blob/master/example/). It covers the basics to get you started with scaling your discord bot. You will need to create a `config.json` and supply your bot's token for the example to run.

# Stats

`<client>.getStats()` outputs a [Stats](https://github.com/arcanebot/redis-sharder/blob/master/src/stats.ts) object. Use this over `<client>.guilds.size`. 
```js
{
  guilds: 9,
  users: 4099,
  estimatedTotalUsers: 4144,
  voice: 0,
  shards: [ { status: 'ready', id: 0, latency: 74, guilds: 9 } ],
  memoryUsage: { heapUsed: 27908672, rss: 83640320 },
  clusters: [
    {
      id: 0,
      shards: [Array],
      guilds: 9,
      unavailableGuilds: 0,
      users: 4099,
      voice: 0,
      memoryUsage: [Object],
      uptime: 229
    }
  ]
}
```
