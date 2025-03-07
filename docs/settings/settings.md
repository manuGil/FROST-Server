---
layout: default
title: Base Settings
category: settings
order: 20
---

# Base Configuration Options

There are several ways to deploy the different packages the make up the FROST-Server.
The HTTP and all-in-one MQTTP packages can be run in Tomcat or Wildfly, or as a docker image.
The MQTT package is a stand-alone application that can be run directly from the command line, or as a docker image.
For each option, the configuration is taken from (in order of priority):

* Tomcat
  1. Enviroment variables
  1. The [Context](http://tomcat.apache.org/tomcat-8.0-doc/config/context.html) entry either in
     * server.xml
     * `$CATALINA_BASE/conf/[enginename]/[hostname]/appname.xml`
     * `/META-INF/context.xml` inside the war file.
  1. web.xml
* Wildfly
  1. Enviroment variables
  1. web.xml
* Standalone MQTT
  1. Enviroment variables
  1. The config file: FrostMqtt.properties

**Important when using Environment variables:** Environment variables are not allowed to have the dot (`.`) character in the name. You must replace all of the dots in the names with an underscore (`_`).


## General Settings

These are settings affecting both the MQTT and HTTP packages.

* **serviceRootUrl:**  
  The base URL of the SensorThings Server without version.
* **defaultCount:**  
  The default value for the $count query option.
* **defaultTop:**  
  The default value for the $top query option.
* **maxTop:**  
  The maximum allowed value for the $top query option.
* **maxDataSize:**  
  The number of bytes that can be loaded before the server stops loading more entities and returns the result. The default is 25000000 (25 MB).
* **useAbsoluteNavigationLinks:**  
  If true, navigationLinks are absolute, otherwise relative.
* **alwaysOrderbyId:**  
  Always add an 'orderby=id asc' to queries to ensure consistent paging.
* **logSensitiveData:**  
  If false, sensitive data like passwords and database connection URLs are not logged when loading settings. Default: `false`.
* **queueLoggingInterval:**  
  If non-zero, log queue statistics ever x milliseconds. Default: 0 (off)


## HTTP settings

These are settings for the HTTP package.

* **http.cors.enable:**  
  If true, a filter is added to allow cross-site-scripting. Default: `false`.
* **http.cors.allowed.origins:**  
  A list of origins that are allowed to access the resource. A * can be specified to enable access to resource
  from any origin. Otherwise, a whitelist of comma separated origins can be provided. Eg: `http://www.w3.org, https://www.apache.org`.
  Default: `*`.
* **http.cors.allowed.methods:**  
  A comma separated list of HTTP methods that can be used to access the resource, using cross-origin requests.
  These are the methods which will also be included as part of Access-Control-Allow-Methods header in pre-flight response.
  Eg: `GET, POST`. Default: `GET, HEAD, OPTIONS`.
* **http.cors.exposed.headers:**  
  A comma separated list of headers other than simple response headers that browsers are allowed to access.
  These are the headers which will also be included as part of Access-Control-Expose-Headers header in the pre-flight response.
  Eg: `X-CUSTOM-HEADER-PING,X-CUSTOM-HEADER-PONG`. Default: `Location`.
* **http.cors.allowed.headers:**  
  A comma separated list of request headers that can be used when making an actual request. These headers will
  also be returned as part of Access-Control-Allow-Headers header in a pre-flight response. Eg: `Origin,Accept`.
  Default: `Origin, Accept, X-Requested-With, Content-Type, Access-Control-Request-Method, Access-Control-Request-Headers, Authorization`.
* **http.cors.support.credentials:**  
  A flag that indicates whether the resource supports user credentials. This flag is exposed as part of
  Access-Control-Allow-Credentials header in a pre-flight response. It helps browser determine whether or not an actual request can
  be made using credentials. Default: `false`.
* **http.cors.preflight.maxage:**  
  The amount of seconds, browser is allowed to cache the result of the pre-flight request. This will be included
  as part of Access-Control-Max-Age header in the pre-flight response. A negative value will prevent CORS Filter from adding this
  response header to pre-flight response. Default: `1800`.
* **http.cors.request.decorate:**  
  A flag to control if CORS specific attributes should be added to HttpServletRequest object or not. Default: `true`.


## Auth settings

See [auth](auth.md) for more information.



## MQTT settings

These are settings for the MQTT package.

* **mqtt.mqttServerImplementationClass:**  
  The java class used for running the MQTT server (must implement MqttServer interface)
* **mqtt.Enabled:**  
  Specifies wether MQTT support will be enabled or not.
* **mqtt.Host:**  
  The external IP address or host name the MQTT server should listen on. Set to 0.0.0.0 to listen on all interfaces.
* **mqtt.internalHost:**  
  The internal host name of the MQTT server.
* **mqtt.Port:**  
  The port the MQTT server runs on.
* **mqtt.sslPort:**  
  The port the MQTT server runs on, using ssl.
* **mqtt.QoS:**  
  Quality of Service Level for MQTT messages. Allowed values: 0, 1, 2. Setting this to 1 or 2 makes it so messages
  are guaranteed to be delivered, but also severely reduces performance and throughput. Default: 2 (exactly once)
* **mqtt.SubscribeMessageQueueSize:**  
  Queue size for messages to be pubslihed via MQTT.
* **mqtt.SubscribeThreadPoolSize:**  
  Number of threads use to dispatch MQTT notifications.
* **mqtt.CreateMessageQueueSize:**  
  Queue size for create observation requests via MQTT.
* **mqtt.CreateThreadPoolSize:**  
  Number of threads use to dispatch observation creation requests.
* **mqtt.WebsocketPort:**  
  The port the MQTT server is reachable via WebSocket.
* **mqtt.secureWebsocketPort:**  
  The port the MQTT server is reachable via secure WebSocket.
* **mqtt.javaKeystorePath:**  
  When using SSL (sslPort or secureWebsocketPort) this is the path to the key store with your certificates.
* **mqtt.keyStorePassword:**  
  The password for the certificate keystore.
* **mqtt.keyManagerPassword:**  
  The password for the certificate itself.
* **mqtt.persistentStoreType:**  
  The way the MQTT server keeps track of subscriptions, either in-memory (`memory`) or using an H2 database (`h2`).
  Default: `memory`.
* **mqtt.session.queue.size:**  
  The size of the internal queue the mqtt broker uses per CPU core.
  Default: `1024`.
* **mqtt.persistent.client.expiration:**  
  The maximum lifetime of disconnected sessions. For example `100s` for 100 seconds.
  Default: `3600s`.
* **mqtt.maxInFlight:**  
  The maximum number of "in-flight" messages to allow when sending notifications.
* **mqtt.netty.mqtt.message_size:**  
  The maximum size of MQTT messages. Default: 8092 (Bytes)
* **mqtt.WaitForEnter:**  
  When true, and running in an interactive console, the FROST-MQTT component will read the keyboard input, and exit
  when the enter key is pressed. When false, the FROST-MQTT component has to be stopped by sending it a TERM Signal.
* **mqtt.exposedEndpoints:**  
  A comma separated list of MQTT endpoints to list on the index page.
  when not present, FROST will try to generate this list itself.
* **mqtt.allowFilter:** Since 2.3.2  
  When true, MQTT topics may contain `$filter` query parameters. Default: false


## Persistence Settings

These settings deal with the database connection, for both the HTTP and MQTT packages.

* **persistence.persistenceManagerImplementationClass:**  
  The java class used for persistence (must implement PersistenceManager interface). Current implementations are:
  * **`de.fraunhofer.iosb.ilt.frostserver.persistence.pgjooq.PostgresPersistenceManager`:**  
    Default value, for PostgreSQL.
* **persistence.autoUpdateDatabase:**  
  Automatically apply database updates.
* **persistence.idGenerationMode:**  
  Determines how entity ids are generated. This can be overridden for each Entity Type. The three allowed values are:
  * **`ServerGeneratedOnly`:**  
    Default value, no client defined ids allowed, database generates ids.
  * **`ServerAndClientGenerated`:**  
    Both, server and client generated ids, are allowed.
  * **`ClientGeneratedOnly`:**  
    Client has to provide @iot.id to create entities.
* **persistence.idGenerationMode.<EntityTypeName>:** Since 2.2.0  
  Determines how entity ids are generated for this Entity Type. This overrides **persistence.idGenerationMode**.
* **persistence.transactionRole:**  
  If true, [SET LOCAL ROLE](https://www.postgresql.org/docs/current/sql-set-role.html)
  is used for each query and set as HTTP user name or `anonymous` for anonymous HTTP users,
  to be used typically with [Row-Level Security](https://www.postgresql.org/docs/current/ddl-rowsecurity.html).
  The PostgreSQL role is not related to the AuthProvider roles security (auth.role.* settings) which is also applied.
  This currently has no effect on MQTT since MQTT subscriptions do not query the database directly.
  Default: `false`.
* **persistence.db.jndi.datasource:**  
  JNDI data source name, used when running in Tomcat/Wildfly.
* **persistence.db.driver:**  
  The Database driver to use when not using JNDI. For PostgreSQL this should be: `org.postgresql.Driver`
* **persistence.db.url:**  
  The database connection url when not using JNDI. Example: `jdbc:postgresql://localhost:5432/sensorthings`
* **persistence.db.username:**  
  The username to use when connecting to the database when not using JNDI.
* **persistence.db.password:**  
  The password to use when connecting to the database when not using JNDI.
* **persistence.db.conn.max:**  
  The maximum number of database connections to use, when not using JNDI.
* **persistence.db.conn.idle.max:**  
  The maximum number of idle database connections to keep open, when not using JNDI.
* **persistence.db.conn.idle.min:**  
  The minimum number of idle database connections to keep open, when not using JNDI.
* **persistence.db.schemaPriority:** Since 2.2.0  
  When searching table definitions, if a table with a given name is found in multiple schemas,
  use this comma-separated list of schemas to determine which table to use.
* **persistence.slowQueryThreshold:**  
  The duration threshold in ms after which queries are considered slow and are logged. Default 200, set to 0 to disable.
* **persistence.queryTimeout:**  
  The maximum duration, in seconds, that a query is allowed to take. Default 0 (no timeout). If
  your FROST instance is behind a reverse proxy that will abort the connection after a certain time, set this to the
  same duration.
* **persistence.countMode:** Since 2.0.0  
  The way to count entities. Allowed values:
  * **`FULL`:** (default) Fully count all entities. Can be very slow on large result sets, but always gives accurate results.
  * **`LIMIT_SAMPLE`:** First do a count, with a limit of `countEstimateThreshold`. If the limit is reached, do an
    estimate using [`TABLESAMPLE (1)`](https://www.postgresql.org/docs/current/sql-select.html). For large result sets
    this is much faster than a full count, but is still guaranteed to give accurate results for low counts.
  * **`SAMPLE_LIMIT`:** First do an estimate using `TABLESAMPLE (1)` and if the estimate is below the threshold, do a
    count with a limit of countEstimateThreshold. This is faster than the above option for large result sets, but
    if the estimate is inaccurate, it can give an incorrect estimate for low counts.
  * **`LIMIT_ESTIMATE`:** First do a count, with a limit of `countEstimateThreshold`. If the limit is reached, do an
    estimate using [`EXPLAIN`](https://wiki.postgresql.org/wiki/Count_estimate). For large result sets
    this is even faster than the TABLESAMPLE estimate, but estimates can be wildly inaccurate for fields that are
    not backed by in index. For low counts this method is still guaranteed to give accurate results.
  * **`ESTIMATE_LIMIT`:** First do an estimate using `EXPLAIN` and if the estimate is below the threshold, do a
    count with a limit of countEstimateThreshold. This is the fastest method, but
    if the estimate is inaccurate, it can give an incorrect estimate for low counts.
* **persistence.countEstimateThreshold:** Since 2.0.0  
  When to switch from counting to estimating. Detailed behaviour depends on the value of
  `persistence.countMode`. Default value: 10000.


## message bus settings

These settings configure the way the HTTP and MQTT packages communicate with each other.

* **bus.busImplementationClass:**  
  The java class that is used to connect to the message bus. Current implementations:
  * **`de.fraunhofer.iosb.ilt.sta.messagebus.InternalMessageBus`:**  
    This internal message bus can be used when all components run in the same JVM, as is the case with the all-in-one MQTTP package.
  * **`de.fraunhofer.iosb.ilt.sta.messagebus.MqttMessageBus`:**  
    This message bus implementation connects to an MQTT server to exchange messages.


### Settings for the Message bus classes

* **`de.fraunhofer.iosb.ilt.sta.messagebus.InternalMessageBus`**  
  This internal message bus can be used when all components run in the same JVM, as is the case with the all-in-one MQTTP package.

  * **bus.workerPoolSize:**  
    The number of worker threads to handle sending messages to the bus.
  * **bus.queueSize:**  
    The size of the message queue to buffer messages to be sent to the bus.

* **`de.fraunhofer.iosb.ilt.sta.messagebus.MqttMessageBus`**  
  This message bus implementation connects to an MQTT server to exchange messages.

  * **bus.mqttBroker:**  
    The MQTT broker to use as a message bus.
  * **bus.sendWorkerPoolSize:**  
    The number of worker threads to handle sending messages to the bus.
  * **bus.sendQueueSize:**  
    The size of the message queue to buffer messages to be sent to the bus.
  * **bus.recvWorkerPoolSize:**  
    The number of worker threads to handle messages coming from the bus.
  * **bus.recvQueueSize:**  
    The size of the message queue to buffer messages coming from the bus.
  * **bus.topicName:**  
    The MQTT topic to use as a message bus.
  * **bus.qosLevel:**  
    The Quality of Service Level for the MQTT bus.
  * **bus.maxInFlight:**  
    The maximum number of "in-flight" messages to allow on the MQTT bus.


## Extension Settings

These settings control various non-standard extensions.

* **extension.customLinks.enable:**  
  Enables the EntityLinking extension described in:
  [https://github.com/INSIDE-information-systems/SensorThingsAPI/blob/master/EntityLinking/Linking.md](https://github.com/INSIDE-information-systems/SensorThingsAPI/blob/master/EntityLinking/Linking.md)
   Default: `false`.
* **extension.customLinks.recurseDepth:**  
  The depth to search for custom links in properties. Default: 0 (only top level)
* **extension.filterDelete.enable:**  
  Enables DELETE on EntitySets, with filters. By default only individual Entities can be deleted. Default: false.


