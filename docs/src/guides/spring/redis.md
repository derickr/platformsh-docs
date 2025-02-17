---
title: "How to Deploy Spring on Platform.sh with Redis"
sidebarTitle: "Redis"
weight: -110
layout: single
description: |
    Configure a Spring application with Redis.
---

To activate Redis and then have it accessed by the Spring application already in Platform.sh, it is necessary to modify two files. 

{{< note >}}
This guide only covers the *addition* of a service configuration to an existing Spring project already configured to deploy on Platform.sh. Please see the [deployment guide](/guides/spring/deploy/_index.md) for more detailed instructions for setting up app containers and initial projects. 
{{< /note >}}

## 1. Add the Redis service

In your [service configuration](../../add-services/_index.md),
include persistent Redis with a [valid supported version](../../add-services/redis.md#persistent-redis):

{{< readFile file="src/registry/images/examples/full/redis-persistent.services.yaml" highlight="yaml" location=".platform/services.yaml" >}}

## 2. Add the Redis relationship

In your [app configuration](../../create-apps/app-reference.md), use the service name `searchelastic` to grant the application access to Elasticsearch via a relationship:

{{< readFile file="src/registry/images/examples/full/redis-persistent.app.yaml" highlight="yaml" location=".platform.app.yaml" >}}

## 3. Export connection credentials to the environment

Connection credentials for Redis, like any service, are exposed to the application container through the `PLATFORM_RELATIONSHIPS` environment variable from the deploy hook onward. Since this variable is a base64 encoded JSON object of all of your project's services, you'll likely want a clean way to extract the information specific to Elasticsearch into it's own environment variables that can be used by Spring. On Platform.sh, custom environment variables can be defined programmatically in a `.environment` file using `jq` to do just that:

```text
export SPRING_REDIS_HOST=$(echo $PLATFORM_RELATIONSHIPS | base64 --decode | jq -r ".redisdata[0].host")
export SPRING_REDIS_PORT=$(echo $PLATFORM_RELATIONSHIPS | base64 --decode | jq -r ".redisdata[0].port")
export JAVA_OPTS="-Xmx$(jq .info.limits.memory /run/config.json)m -XX:+ExitOnOutOfMemoryError"
```

{{< note title="Tip" >}}

{{% spring-common-props %}}

{{< /note >}}

## 4. Connect to Redis

Commit that code and push. The Redis instance is ready to be connected from within the Spring application.

## Use Spring Data for Redis

You can use [Spring Data Redis](https://spring.io/projects/spring-data-mongodb) to use Redis with your app.
First, determine the MongoDB client using the [Java configuration reader library](https://github.com/platformsh/config-reader-java).

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.data.redis.connection.jedis.JedisConnectionFactory;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.serializer.GenericToStringSerializer;

@Configuration
public class RedisConfig {


    @Bean
    JedisConnectionFactory jedisConnectionFactory() {
        Config config = new Config();
        RedisSpring redis = config.getCredential("redis", RedisSpring::new);
        return redis.get();
    }

    @Bean
    public RedisTemplate<String, Object> redisTemplate() {
        final RedisTemplate<String, Object> template = new RedisTemplate<String, Object>();
        template.setConnectionFactory(jedisConnectionFactory());
        template.setValueSerializer(new GenericToStringSerializer<Object>(Object.class));
        return template;
    }

}
```
