---
title: Spring WebFlux with WebSockets for Real-time Data
date: 2023-01-22 12:30:00 +0530
categories: [Spring WebFlux, WebSockets]
tags: [java, springboot, spring-webflux, websockets]
---

## **Spring WebFlux with WebSockets for Real-time Data**

In this tutorial, we will learn how to use Spring WebFlux to stream real-time sensor data to a web client using WebSockets. We'll start by creating a simple Spring Boot application and then implement a WebSocket endpoint to stream the sensor data.

### **Setting up the Project**

To get started, we'll create a new Spring Boot project with the WebFlux dependency:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-webflux</artifactId>
</dependency>
```

### **Creating the WebSocket Endpoint**

Now that we have the dependencies set up, we can create a **`SensorDataHandler`** class that will handle the WebSocket connections and stream the sensor data:

```java
@Component
public class SensorDataHandler implements WebSocketHandler {

    private final Flux<String> sensorData;

    public SensorDataHandler() {
        this.sensorData = Flux.interval(Duration.ofSeconds(1))
            .map(s -> s.toString())
            .share();
    }

    @Override
    public Mono<Void> handle(WebSocketSession session) {
    	return session.send(sensorData.map(session::textMessage));
    }
}
```

In this example, the **`SensorDataHandler`** class creates a **`Flux`** of sensor data that is emitted every second. In this example, the data is just a sequence of long numbers incrementing every second. The **`handle`** method of the **`SensorDataHandler`** overrides the method from `**WebSocketHandler**` and returns the data. The client can connect to this endpoint using a WebSocket and receive the sensor data in real-time.

### **Configuring the WebSocket Endpoint**

To configure the WebSocket endpoint, we'll create a **`WebSocketConfig`** class and define a **`HandlerMapping`** to map the **`handle`** method to the **`/sensors`** endpoint:

```java
@Configuration
public class WebSocketConfig {

	@Bean
	HandlerMapping webSocketHandlerMapping(SensorDataHandler handler) {
	    Map<String, WebSocketHandler> map = new HashMap<>();
	    map.put("/sensors", handler);

	    SimpleUrlHandlerMapping handlerMapping = new SimpleUrlHandlerMapping();
	    handlerMapping.setOrder(1);
	    handlerMapping.setUrlMap(map);
	    return handlerMapping;
	}
}
```

In this class we have a **`webSocketHandlerMapping`** method that maps the **`handle`** method of the **`SensorDataHandler`** class to the **`/sensors`** endpoint. 

### **Testing the WebSocket Endpoint**

To test the WebSocket endpoint, we'll create a simple HTML page with JavaScript code to connect to the WebSocket endpoint and display the sensor data in real-time. Here's an example of how to do this using the **`WebSocket`** API:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Sensor Data</title>
</head>
<body>
    <div id="sensor-data"></div>
    <script>
        var socket = new WebSocket("ws://localhost:8080/sensors");
        socket.onmessage = function(event) {
            var data = JSON.parse(event.data);
            var div = document.getElementById("sensor-data");
            div.innerHTML += " value: " + data + "<br>";
        };
    </script>
</body>
</html>
```

This code creates a new **`WebSocket`** object and opens a connection to the **`/sensors`** endpoint. When new sensor data is received, the **`onmessage`** callback function is called, which parses the JSON data and displays it on the page.

### **Conclusion**

In this tutorial, we've learned how to use Spring WebFlux to stream real-time sensor data to a web client using WebSockets. We've seen how to create a simple Spring Boot application and implement a WebSocket endpoint to stream the sensor data. We also saw how to configure the WebSocket endpoint and test it using a simple HTML page with JavaScript code. 

You can find the full code in this [GitHub Repository](https://github.com/rai-sandeep/spring-webflux/tree/main/web-socket).