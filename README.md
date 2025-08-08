# Spring Data Redis

## Tabla de contenido

- [Introducción a Redis y Spring Data Redis](#introduccion-redis-spring-data-redis)
  - [¿Qué es Redis?](#que-es-redis)
    - [Características Principales (Key-Value, In-Memory, Persistencia)](#caracteristicas-principales-redis)
    - [Tipos de Datos de Redis (Strings, Hashes, Lists, Sets, Sorted Sets)](#tipos-datos-redis)
    - [Casos de Uso Comunes de Redis](#casos-uso-comunes-redis)
    - [Modelo de Consistencia y Atomicidad](#modelo-consistencia-atomicidad-redis)
  - [¿Qué es Spring Data Redis?](#que-es-spring-data-redis)
    - [Propósito y Arquitectura](#proposito-arquitectura-spring-data-redis)
    - [Ventajas de usar Spring Data Redis](#ventajas-spring-data-redis)
  - [Configuración Inicial del Proyecto](#configuracion-inicial-spring-data-redis)
    - [Dependencias (Maven/Gradle)](#dependencias-spring-data-redis)
    - [Configuración de Conexión (`application.properties/yml`)](#configuracion-conexion-redis)
    - [Configuración de `RedisTemplate` y `ReactiveRedisTemplate`](#configuracion-redistemplate)
    - [Conexión con Clientes (Jedis, Lettuce)](#clientes-jedis-lettuce)

- [Operaciones Básicas con `RedisTemplate`](#operaciones-basicas-redistemplate)
  - [Serialización de Datos](#serializacion-datos-redis)
    - [`StringRedisSerializer`](#stringredisserializer)
    - [`JdkSerializationRedisSerializer`](#jdkserializationredisserializer)
    - [`GenericJackson2JsonRedisSerializer`](#genericjackson2jsonredisserializer)
    - [`Jackson2JsonRedisSerializer`](#jackson2jsonredisserializer)
    - [Estrategias de Serialización y Deserialización](#estrategias-serializacion-deserializacion)
  - [Operaciones de Clave-Valor (Value Operations)](#operaciones-clave-valor)
    - [`opsForValue()` (Strings)](#opsforvalue)
  - [Operaciones de Hashes (`opsForHash()`)](#operaciones-hashes)
  - [Operaciones de Listas (`opsForList()`)](#operaciones-listas)
  - [Operaciones de Conjuntos (`opsForSet()`)](#operaciones-conjuntos)
  - [Operaciones de Conjuntos Ordenados (`opsForZSet()`)](#operaciones-conjuntos-ordenados)
  - [Manejo de Expiración (TTL)](#manejo-expiracion-redis)
  - [Transacciones Redis (Pipelining y Multi/Exec)](#transacciones-redis)
    - [`sessionCallback()`](#sessioncallback)
    - [Uso de `multi()` y `exec()`](#multi-exec)
    - [Ventajas y Limitaciones de las Transacciones Redis](#ventajas-limitaciones-transacciones-redis)

- [Repositorios de Spring Data Redis](#repositorios-spring-data-redis)
  - [`CrudRepository` para Redis](#crudrepository-redis)
    - [Anotaciones de Mapeo (`@RedisHash`, `@Id`, `@Reference`)](#anotaciones-mapeo-redis)
    - [Definición de Consultas Derivadas de Métodos](#consultas-derivadas-redis)
  - [`ReactiveRedisRepository` (para WebFlux)](#reactiveredisrepository)
    - [Configuración y Uso Reactivo](#configuracion-uso-reactivo-redis)
    - [Consideraciones de Rendimiento Reactivo](#consideraciones-rendimiento-reactivo-redis)

- [Redis como Caché con Spring Cache](#redis-como-cache-spring)
  - [Habilitar Caché en Spring Boot (`@EnableCaching`)](#enablecaching-redis)
  - [Anotaciones de Caché (`@Cacheable`, `@CachePut`, `@CacheEvict`)](#anotaciones-cache-redis)
  - [Configuración de Caché con Redis](#configuracion-cache-redis)
    - [`RedisCacheConfiguration`](#rediscacheconfiguration)
    - [TTL por clave de caché](#ttl-cache-redis)
  - [Estrategias de Cacheo (Read-Through, Write-Through, Write-Behind)](#estrategias-cacheo-redis)
  - [Manejo de Consistencia y Evicción](#manejo-consistencia-eviccion-redis)

- [Publicación y Suscripción (Pub/Sub)](#publicacion-suscripcion-pub-sub)
  - [Conceptos de Pub/Sub en Redis](#conceptos-pub-sub-redis)
  - [Configuración de Listeners](#configuracion-listeners-redis)
    - [`MessageListenerAdapter`](#messagelisteneradapter)
    - [`RedisMessageListenerContainer`](#redismessagelistenercontainer)
  - [Envío de Mensajes (`RedisTemplate.convertAndSend()`)](#envio-mensajes-pub-sub)
  - [Manejo de Canales y Patrones](#manejo-canales-patrones)
  - [Pub/Sub Reactivo (`ReactiveRedisTemplate.listen()`)](#pub-sub-reactivo)

- [Características Avanzadas y Casos de Uso Específicos](#caracteristicas-avanzadas-casos-uso)
  - [Atomic Counters y Distribuited Locks](#atomic-counters-distribuited-locks)
    - [`RedisAtomicLong`](#redisatomiclong)
    - [`RedisLockRegistry`](#redislockregistry)
    - [Implementación de Bloqueos Distribuidos](#implementacion-bloqueos-distribuidos)
  - [Geospatial Data (Datos Geoespaciales)](#geospatial-data-redis)
    - [`opsForGeo()`](#opsforgeo)
    - [Casos de Uso (ej. "Encuentra lo más cercano")](#casos-uso-geospatial)
  - [HyperLogLog](#hyperloglog-redis)
    - [`opsForHyperLogLog()`](#opsforhyperloglog)
    - [Conteo de elementos únicos aproximado](#conteo-elementos-unicos)
  - [Stream Processing (Streams de Redis)](#stream-processing-redis)
    - [`opsForStream()`](#opsforstream)
    - [Consumer Groups](#consumer-groups-redis)
    - [Casos de Uso (Colas de Mensajes Persistentes)](#casos-uso-stream-redis)
  - [Spring Session con Redis](#spring-session-redis)
    - [Almacenamiento de Sesiones HTTP en Redis](#almacenamiento-sesiones-http)
    - [Configuración y Ventajas](#configuracion-ventajas-spring-session)
  - [Transacciones Multi-Clave (Scripts Lua)](#transacciones-multi-clave-lua)
    - [`RedisScript`](#redisscript)
    - [Ventajas de Atomicidad y Rendimiento](#ventajas-atomicidad-rendimiento-lua)
  - [Redis Sentinel y Cluster](#redis-sentinel-cluster)
    - [Alta Disponibilidad con Sentinel](#alta-disponibilidad-sentinel)
    - [Escalabilidad y Sharding con Cluster](#escalabilidad-sharding-cluster)
    - [Configuración en Spring Data Redis](#configuracion-sentinel-cluster)

- [Testing en Spring Data Redis](#testing-spring-data-redis)
  - [Pruebas Unitarias y de Integración](#pruebas-unitarias-integracion-redis)
  - [Uso de Redis Embeddable (ej. `redis-unit`)](#redis-embeddable)

- [Mejores Prácticas y Rendimiento](#mejores-practicas-rendimiento-redis)
  - [Manejo de Memoria y Evicción de Datos](#manejo-memoria-eviccion-redis)
  - [Monitoreo y Métricas](#monitoreo-metricas-redis)
  - [Consideraciones de Red y Latencia](#consideraciones-red-latencia-redis)
  - [Estrategias de Persistencia (RDB, AOF)](#estrategias-persistencia-redis)


<a id="introduccion-redis-spring-data-redis"></a>
## Introducción a Redis y Spring Data Redis

<a id="que-es-redis"></a>
### ¿Qué es Redis?

<a id="caracteristicas-principales-redis"></a>
#### Características Principales (Key-Value, In-Memory, Persistencia)

<a id="tipos-datos-redis"></a>
#### Tipos de Datos de Redis (Strings, Hashes, Lists, Sets, Sorted Sets)

<a id="casos-uso-comunes-redis"></a>
#### Casos de Uso Comunes de Redis

<a id="modelo-consistencia-atomicidad-redis"></a>
#### Modelo de Consistencia y Atomicidad

<a id="que-es-spring-data-redis"></a>
### ¿Qué es Spring Data Redis?

<a id="proposito-arquitectura-spring-data-redis"></a>
#### Propósito y Arquitectura

<a id="ventajas-spring-data-redis"></a>
#### Ventajas de usar Spring Data Redis

<a id="configuracion-inicial-spring-data-redis"></a>
### Configuración Inicial del Proyecto

<a id="dependencias-spring-data-redis"></a>
#### Dependencias (Maven/Gradle)

<a id="configuracion-conexion-redis"></a>
#### Configuración de Conexión (`application.properties/yml`)

<a id="configuracion-redistemplate"></a>
#### Configuración de `RedisTemplate` y `ReactiveRedisTemplate`

<a id="clientes-jedis-lettuce"></a>
#### Conexión con Clientes (Jedis, Lettuce)

<a id="operaciones-basicas-redistemplate"></a>
## Operaciones Básicas con `RedisTemplate`

<a id="serializacion-datos-redis"></a>
### Serialización de Datos

<a id="stringredisserializer"></a>
#### `StringRedisSerializer`

<a id="jdkserializationredisserializer"></a>
#### `JdkSerializationRedisSerializer`

<a id="genericjackson2jsonredisserializer"></a>
#### `GenericJackson2JsonRedisSerializer`

<a id="jackson2jsonredisserializer"></a>
#### `Jackson2JsonRedisSerializer`

<a id="estrategias-serializacion-deserializacion"></a>
#### Estrategias de Serialización y Deserialización

<a id="operaciones-clave-valor"></a>
### Operaciones de Clave-Valor (Value Operations)

<a id="opsforvalue"></a>
#### `opsForValue()` (Strings)

<a id="operaciones-hashes"></a>
### Operaciones de Hashes (`opsForHash()`)

<a id="operaciones-listas"></a>
### Operaciones de Listas (`opsForList()`)

<a id="operaciones-conjuntos"></a>
### Operaciones de Conjuntos (`opsForSet()`)

<a id="operaciones-conjuntos-ordenados"></a>
### Operaciones de Conjuntos Ordenados (`opsForZSet()`)

<a id="manejo-expiracion-redis"></a>
### Manejo de Expiración (TTL)

<a id="transacciones-redis"></a>
### Transacciones Redis (Pipelining y Multi/Exec)

<a id="sessioncallback"></a>
#### `sessionCallback()`

<a id="multi-exec"></a>
#### Uso de `multi()` y `exec()`

<a id="ventajas-limitaciones-transacciones-redis"></a>
#### Ventajas y Limitaciones de las Transacciones Redis

<a id="repositorios-spring-data-redis"></a>
## Repositorios de Spring Data Redis

<a id="crudrepository-redis"></a>
### `CrudRepository` para Redis

<a id="anotaciones-mapeo-redis"></a>
#### Anotaciones de Mapeo (`@RedisHash`, `@Id`, `@Reference`)

<a id="consultas-derivadas-redis"></a>
#### Definición de Consultas Derivadas de Métodos

<a id="reactiveredisrepository"></a>
### `ReactiveRedisRepository` (para WebFlux)

<a id="configuracion-uso-reactivo-redis"></a>
#### Configuración y Uso Reactivo

<a id="consideraciones-rendimiento-reactivo-redis"></a>
#### Consideraciones de Rendimiento Reactivo

<a id="redis-como-cache-spring"></a>
## Redis como Caché con Spring Cache

<a id="enablecaching-redis"></a>
### Habilitar Caché en Spring Boot (`@EnableCaching`)

<a id="anotaciones-cache-redis"></a>
### Anotaciones de Caché (`@Cacheable`, `@CachePut`, `@CacheEvict`)

<a id="configuracion-cache-redis"></a>
### Configuración de Caché con Redis

<a id="rediscacheconfiguration"></a>
#### `RedisCacheConfiguration`

<a id="ttl-cache-redis"></a>
#### TTL por clave de caché

<a id="estrategias-cacheo-redis"></a>
### Estrategias de Cacheo (Read-Through, Write-Through, Write-Behind)

<a id="manejo-consistencia-eviccion-redis"></a>
### Manejo de Consistencia y Evicción

<a id="publicacion-suscripcion-pub-sub"></a>
## Publicación y Suscripción (Pub/Sub)

<a id="conceptos-pub-sub-redis"></a>
### Conceptos de Pub/Sub en Redis

<a id="configuracion-listeners-redis"></a>
### Configuración de Listeners

<a id="messagelisteneradapter"></a>
#### `MessageListenerAdapter`

<a id="redismessagelistenercontainer"></a>
#### `RedisMessageListenerContainer`

<a id="envio-mensajes-pub-sub"></a>
### Envío de Mensajes (`RedisTemplate.convertAndSend()`)

<a id="manejo-canales-patrones"></a>
### Manejo de Canales y Patrones

<a id="pub-sub-reactivo"></a>
### Pub/Sub Reactivo (`ReactiveRedisTemplate.listen()`)

<a id="caracteristicas-avanzadas-casos-uso"></a>
## Características Avanzadas y Casos de Uso Específicos

<a id="atomic-counters-distribuited-locks"></a>
### Atomic Counters y Distribuited Locks

<a id="redisatomiclong"></a>
#### `RedisAtomicLong`

<a id="redislockregistry"></a>
#### `RedisLockRegistry`

<a id="implementacion-bloqueos-distribuidos"></a>
#### Implementación de Bloqueos Distribuidos

<a id="geospatial-data-redis"></a>
### Geospatial Data (Datos Geoespaciales)

<a id="opsforgeo"></a>
#### `opsForGeo()`

<a id="casos-uso-geospatial"></a>
#### Casos de Uso (ej. "Encuentra lo más cercano")

<a id="hyperloglog-redis"></a>
### HyperLogLog

<a id="opsforhyperloglog"></a>
#### `opsForHyperLogLog()`

<a id="conteo-elementos-unicos"></a>
#### Conteo de elementos únicos aproximado

<a id="stream-processing-redis"></a>
### Stream Processing (Streams de Redis)

<a id="opsforstream"></a>
#### `opsForStream()`

<a id="consumer-groups-redis"></a>
#### Consumer Groups

<a id="casos-uso-stream-redis"></a>
#### Casos de Uso (Colas de Mensajes Persistentes)

<a id="spring-session-redis"></a>
### Spring Session con Redis

<a id="almacenamiento-sesiones-http"></a>
#### Almacenamiento de Sesiones HTTP en Redis

<a id="configuracion-ventajas-spring-session"></a>
#### Configuración y Ventajas

<a id="transacciones-multi-clave-lua"></a>
### Transacciones Multi-Clave (Scripts Lua)

<a id="redisscript"></a>
#### `RedisScript`

<a id="ventajas-atomicidad-rendimiento-lua"></a>
#### Ventajas de Atomicidad y Rendimiento

<a id="redis-sentinel-cluster"></a>
### Redis Sentinel y Cluster

<a id="alta-disponibilidad-sentinel"></a>
#### Alta Disponibilidad con Sentinel

<a id="escalabilidad-sharding-cluster"></a>
#### Escalabilidad y Sharding con Cluster

<a id="configuracion-sentinel-cluster"></a>
#### Configuración en Spring Data Redis

<a id="testing-spring-data-redis"></a>
## Testing en Spring Data Redis

<a id="pruebas-unitarias-integracion-redis"></a>
### Pruebas Unitarias y de Integración

<a id="redis-embeddable"></a>
### Uso de Redis Embeddable (ej. `redis-unit`)

<a id="mejores-practicas-rendimiento-redis"></a>
## Mejores Prácticas y Rendimiento

<a id="manejo-memoria-eviccion-redis"></a>
### Manejo de Memoria y Evicción de Datos

<a id="monitoreo-metricas-redis"></a>
### Monitoreo y Métricas

<a id="consideraciones-red-latencia-redis"></a>
### Consideraciones de Red y Latencia

<a id="estrategias-persistencia-redis"></a>
### Estrategias de Persistencia (RDB, AOF)
