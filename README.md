### a. How much data does your publisher program send to the message broker in one run?

In one run, the publisher program sends five messages to the message broker. Each message is a serialized UserCreatedEventMessage struct containing two fields: a user_id (string) and a user_name (string). Since each field is a short string and the message uses Borsh serialization (which is binary and compact), the total data size per message is relatively small—likely around a few dozen bytes per message. Thus, across five messages, the total data sent is approximately a few hundred bytes, depending on the length of the strings used.

### b. The URL “amqp://guest:guest@localhost:5672” is the same as in the subscriber program. What does it mean?

The URL amqp://guest:guest@localhost:5672 indicates that both the publisher and subscriber are connecting to the same RabbitMQ message broker using the same access credentials. The first guest is the username, and the second guest is the password for authentication. localhost means that the broker is running on the local machine, and 5672 is the default port used by RabbitMQ for AMQP protocol communication. Using the same URL ensures that both components (publisher and subscriber) are interacting with the same message exchange system, enabling successful message delivery and handling.

### Running RabbitMQ as message broker

![Image](https://github.com/user-attachments/assets/38e2220a-6ab2-47a8-a492-83ae1acff2cd)

### Sending and processing event

### Publisher console

![Image](https://github.com/user-attachments/assets/8ff6e7cf-bbe0-4165-8459-1a17e955f739)

### Subscriber console

![Image](https://github.com/user-attachments/assets/02a19906-cfe8-4c92-9e0c-e39b9f881c48)

Description:<br>

After installing Docker Desktop and running RabbitMQ with the provided Docker command, I tested the publisher–subscriber system built using Rust and the `crosstown_bus` crate.

I opened two terminals: one for the subscriber and one for the publisher. I first ran the subscriber using `cargo run`, which connected to the RabbitMQ broker successfully. This was confirmed by checking the RabbitMQ management UI at `http://localhost:15672`, where one active connection appeared.

Next, I ran the publisher using `cargo run` in the publisher directory. The publisher sent five `UserCreatedEventMessage` events, each containing a user ID and a name that includes my NPM. These messages were received and printed out by the subscriber in real time, showing that the message queue and delivery system worked correctly.

Each time the publisher is run, new events are sent to the message broker and immediately consumed by the subscriber. This confirms that the AMQP connection, message serialization (using Borsh), and queue system are functioning as expected.

### Monitoring chart based on publisher

![Image](https://github.com/user-attachments/assets/324b9922-cfd3-4014-9934-201b69fac919)

Description:<br>


When the publisher is executed using `cargo run`, it sends five `UserCreatedEventMessage` events to the RabbitMQ message broker. Each event is published to the `user_created` queue, which is being listened to by the subscriber.

If we observe the RabbitMQ Management UI at `http://localhost:15672`, we can see the **"Message rates"** chart showing a spike every time the publisher runs. This spike represents the burst of messages being published and delivered in a very short time. Since all five messages are sent almost instantly, the spike is sharp but short-lived.

These spikes are a visual confirmation that the publisher is actively sending messages to the broker, and that the broker is delivering them to the subscriber efficiently. Re-running the publisher will produce additional spikes on the chart, depending on the message rate and delivery confirmation.

