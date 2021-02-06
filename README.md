# WebSocket

## STOMP

- Streaming Text Oriented Messaging Protocol

- In GreetingController class
	The ```@MessageMapping``` annotation ensures that, if a message is sent to the ```/hello``` destination, the ```greeting()``` method is called.

	The payload of the message is bound to a HelloMessage object, which is passed into greeting().

	After the processing, the ```greeting()``` method creates a Greeting object and returns it. The return value is broadcast to all subscribers of ```/topic/greetings```, as specified in the @SendTo annotation.

- In WebSocketCongig class

	```WebSocketConfig``` is annotated with ```@Configuration``` to indicate that it is a Spring configuration class. It is also annotated with @EnableWebSocketMessageBroker. As its name suggests, ```@EnableWebSocketMessageBroker``` enables WebSocket message handling, backed by a message broker.

	The ```configureMessageBroker()``` method implements the default method in WebSocketMessageBrokerConfigurer to configure the message broker. It starts by calling ```enableSimpleBroker()``` to enable a simple memory-based message broker to carry the greeting messages back to the client on destinations prefixed with ```/topic```. It also designates the ```/app``` prefix for messages that are bound for methods annotated with @MessageMapping. This prefix will be used to define all the message mappings. For example, ```/app/hello``` is the endpoint that the GreetingController.greeting() method is mapped to handle.

	The ```registerStompEndpoints()``` method registers the ```/websocket``` endpoint, enabling SockJS fallback options so that alternate transports can be used if WebSocket is not available. The SockJS client will attempt to connect to ```/websocket``` and use the best available transport (websocket, xhr-streaming, xhr-polling, and so on).