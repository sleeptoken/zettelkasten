
2024-12-28 22:55

Source: #AOC24 #hardware

Tags: [[Networking]] [[Wireshark]] [[Hardware]]

- MQTT stands for Message Queuing Telemetry Transport.
- It is a language very commonly used in IoT devices for communication purposes. It works on a publish/subscribe model, where any client device can publish messages, and other client devices can subscribe to the messages if they are related to a topic of interest. 

**MQTT Clients:** MQTT clients are IoT devices, such as sensors and controllers, that publish or subscribe to messages using the MQTT protocol. 
For example,
	a temperature sensor can be a client that publishes temperature sensors at different places. An HVAC controller can also act as a client that subscribes to messages from the temperature sensor and turns the HVAC system on or off based on the input received.

**MQTT Broker:** An MQTT broker receives messages from publishing clients and distributes them to the subscribing clients based on their preferences.

**MQTT Topics:** Topics are used to classify the different types of messages. Clients can subscribe to messages based on their topics of interest. 
### Lab

- keep the IoT device running. 
- on seeing the MQTT filtered traffic in Wireshark we see a lot of messages which have publish and subscribe messages 
- we find the topic name and the message that means "on" from the Wireshark dump

to publish a message to the MQTT broker in order to turn on something.
```
mosquitto_pub -h localhost -t "some_topic" -m "message"
```
- `mosquitto_pub` is the command-line utility to publish an MQTT message
- `-h localhost` refers to the MQTT broker, which is `localhost` in this task
### References
Task 30