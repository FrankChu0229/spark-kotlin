# spark-kotlin

A Spark DSL in idiomatic kotlin.

Authors:
--------
- Per Wendel, @perwendel
- Love Löfdahl, @lallemupp

Dependency:
-----------
Maven:
```xml
<dependency>
    <groupId>com.sparkjava</groupId>
    <artifactId>spark-kotlin</artifactId>
    <version>1.0.0-alpha</version>
</dependency>
```

Gradle:
```groovy
compile "com.sparkjava:spark-kotlin:1.0.0-alpha"
```

Documentation
-------------

Routes
------

```kotlin
// Static API
get("/hello") {
    "Hello Spark Kotlin"
}

get("/nothing") {
    status(404)
    "Oops, we couldn't find what you're looking for"
}

get("/saymy/:name") {
    params(":name")
}

get("/redirect") {
    redirect("/hello")
}

// Instance API
val http = ignite()

http.get("/hello") {
    "Hello Spark Kotlin"
}

http.get("/nothing") {
    status(404)
    "Oops, we couldn't find what you're looking for"
}

http.get("/saymy/:name") {
    params(":name")
}

http.get("/redirect") {
    redirect("/hello")
}
```
Initialization DSL
------------------
```kotlin
// Static API
config {
    port = 5500
    ipAddress = "0.0.0.0"
    threadPool {
        maxThreads = 10
        minThreads = 5
        idleTimeoutMillis = 1000
    }
    secure {
        keystore {
            file = keyStoreLocation()
            password = keystorePass()
        }
        truststore {
            file = trustStoreLocation()
            password = trustStorePassword()
        }
        needsClientCert = false
    }
    staticFiles {
        location = "/public"
        expiryTime = 36000.seconds
        headers(
                "description" to "static content",
                "licence" to "free to use"
        )
        mimeTypes(
                "cxt" to "text/html"
        )
    }
}

// Instance API
val http = ignite {
    port = 5500
    ipAddress = "0.0.0.0"
    threadPool {
        maxThreads = 10
        minThreads = 5
        idleTimeoutMillis = 1000
    }
    secure {
        keystore {
            file = keyStoreLocation()
            password = keystorePass()
        }
        truststore {
            file = trustStoreLocation()
            password = trustStorePassword()
        }
        needsClientCert = false
    }
    staticFiles {
        location = "/public"
        expiryTime = 36000.seconds
        headers(
                "description" to "static content",
                "licence" to "free to use"
        )
        mimeTypes(
                "cxt" to "text/html"
        )
    }
}
```
Redirect DSL
------------
```kotlin
// Static API
redirect {
    any(
            "/from" to "/hello",
            "/hi" to "/hello"
    )
    get(
            "/source" to "/target"
    )
    post(
            "/gone" to "/new"
    )
}

// Instance API
http.redirect {
    any(
            "/from" to "/hello",
            "/hi" to "/hello"
    )
    get(
            "/source" to "/target"
    )
    post(
            "/gone" to "/new"
    )
}



```
WebSocket DSL (design ongoing)
------------------------------
```kotlin
// Static API
webSocket("/echo") {
    opened {
        println("[Opened] remote address = " + session.remoteAddress)
    }
    received {
        println("[Received] message = $message")
    }
    closed {
        println("[Closed] code = $code, reason = $reason, session = $session")
    }
    error {
        println("[Error] cause = " + cause)
    }
}

// Instance API
http.webSocket("/echo") {
    opened {
        println("[Opened] remote address = " + session.remoteAddress)
    }
    received {
        println("[Received] message = $message")
    }
    closed {
        println("[Closed] code = $code, reason = $reason, session = $session")
    }
    error {
        println("[Error] cause = " + cause)
    }
}

class WebSocketSession
    val remote
    var idleTimeout
    val isOpen
    val localAddress
    val protocolVersion
    val upgradeResponse
    val upgradeRequest
    val policy
    val isSecure
    val remoteAddress
    
    fun disconnect()
    fun suspend(): Suspension 
    fun close()
    fun close(closeStatus: CloseStatus?)
    fun close(statusCode: Int, reason: String?)
    fun raw(): Session
```