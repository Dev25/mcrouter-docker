mcrouter-docker
---

This is a slim ubuntu 16.04 based Docker image for [Mcrouter](https://github.com/facebook/mcrouter), a popular memcache router. It speaks the memcached ASCII protocol and allows you to create memcached clusters (replicated or sharded) along with various features ranging from failover to flexible key routing. The [introduction to mcrouter](https://code.facebook.com/posts/296442737213493) blog post is a good read to learn more.

The base image is `gcr.io/google_containers/ubuntu-slim`. Further bloat is removed by removing build dependencies and striping out symbols from the binaries/folly lib.

Usage
---
To run mcrouter you need to specific both a port number to listen on `-p <port>` and a config provided via either a file `-f /path/to/config/file.json`  or string `--config-str=<json>`. Refer to the [mcrouter wiki](https://github.com/facebook/mcrouter/wiki/Command-line-options) for more command line arguments.

Example:

The docker-compose config runs mcrouter using the following config where `cache` references a memcache container.
```
mcrouter --config-str='{"pools":{"A":{"servers":["cache:11211"]}},"route":"PoolRoute|A"}' -p 5000
```

```
 ❯❯❯ docker-compose up
Creating network "mcrouterdocker_default" with the default driver
Creating mcrouterdocker_cache_1
Creating mcrouterdocker_mcrouter_1
Attaching to mcrouterdocker_cache_1, mcrouterdocker_mcrouter_1
mcrouter_1  | I0418 19:51:03.447618     1 main.cpp:342] mcrouter --config-str={"pools":{"A":{"servers":["cache:11211"]}},"route":"PoolRoute|A"} -p 5000
mcrouter_1  | I0418 19:51:03.448180     1 main.cpp:468] 35.0.0-master mcrouter startup (1)
mcrouter_1  | I0418 19:51:03.450022     1 main.cpp:368] Starting Memcache router
mcrouter_1  | I0418 19:51:03.450181     1 Server-inl.h:116] Spawning AsyncMcServer
mcrouter_1  | I0418 19:51:03.463030     1 CarbonRouterInstance-inl.h:533] started reconfiguring
mcrouter_1  | I0418 19:51:03.463982     1 CarbonRouterInstance-inl.h:552] reconfigured 1 proxies with 1 pools, 1 clients ad9d4219101afbba58e67c4cf749955b)

```
