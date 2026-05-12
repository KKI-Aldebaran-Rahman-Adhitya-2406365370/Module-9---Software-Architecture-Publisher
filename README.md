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
