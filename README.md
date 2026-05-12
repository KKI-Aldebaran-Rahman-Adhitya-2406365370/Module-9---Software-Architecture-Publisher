# How much data your publisher program will send to the message broker in one run?
The publisher program is configured to send exactly five different event messages to the 
message broker during a single execution. Every message is an instance of the 
`UserCreatedEventMessage` struct which contains a unique `user_id` and a `user_name` string that 
includes my NPM/student ID. These five calls to the `publish_event` method ensure that 
five separate data packets are transmitted over the network to the RabbitMQ service. 
When transmitted, the broker places these events into the "user_created" queue where 
they stay until a subscriber is ready to process them. This fixed amount of data allows 
for straightforward monitoring of the system's initial performance and throughput. 
In a production environment, this volume would typically scale to handle thousands of 
events per second based on user activity.

# The url of: “amqp://guest:guest@localhost:5672” is the same as in the subscriber program, what does it mean?
Using the identical URL "amqp://guest:guest@localhost:5672" in both programs is important 
since it specifies the unique network address of the message broker they must both access. 
Since event-driven architecture relies on an intermediary bus, both the publisher and the 
subscriber must connect to the exact same broker instance to exchange data. The "localhost" 
portion of the string signifies that both programs are communicating with a RabbitMQ service 
hosted on the same machine. This consistency ensures that when the publisher sends an event 
to the "user_created" exchange, the subscriber is already monitoring the same queue 
at that same location. This shared connection string effectively creates the bridge that 
allows these two independent processes to collaborate without knowing each other's internal 
logic. It serves as the single point of truth for the entire messaging infrastructure in 
this tutorial setup.

# Screenshot of "Running RabbitMQ as message broker." commit
![RabbitMQImage1.png](RabbitMQImage1.png)

# Screenshot of "Sending and processing event."
![RabbitMQImage2.png](RabbitMQImage2.png)

![RabbitMQImage3.png](RabbitMQImage3.png)

![RabbitMQImage4.png](RabbitMQImage4.png)

I successfully ran both the subscriber and publisher programs simultaneously to observe their interaction through the RabbitMQ broker. 
When the publisher was executed using cargo run, it broadcasted five different "user_created" events where each event contains a unique user ID and my NPM/student ID. 
The RabbitMQ management console showed an immediate spike in activity since these messages were received and placed into the designated queue. 
On the subscriber side, the console immediately began printing the incoming data which confirms that the listener was effectively pulling messages from the broker in real-time. 
This experiment shows the core benefit of event-driven architecture because publisher was able to send all five messages without needing to know if the subscriber was ready or even active. 
The seamless delivery of data across two independent processes proves that the system is correctly decoupled and communicating through the shared event bus.

# Screenshot of "Monitoring chart based on publisher." commit

![RabbitMQImage5.png](RabbitMQImage5.png)

After executing the publisher program repeatedly, I saw spikes within the RabbitMQ Management "Message rates" and "Queued messages" charts. 
These spikes represent the immediate influx of data as the publisher rapidly broadcasts multiple batches of "user_created" events to the exchange. 
The upward movement on the graph directly correlates with the moment the `cargo run` command is executed which means that the broker is actively receiving and routing new messages. 
Because the subscriber processes these messages almost instantly in the current setup, the spikes are sharp and subside quickly once the publisher finishes its five-message cycle.
Monitoring these charts is an important practice in software architecture because it allows developers to visualize the system's throughput and identify potential bottlenecks in real-time.
By observing these patterns, I think the message broker is effectively handling the burst of traffic and maintaining the flow of communication between my decoupled services.