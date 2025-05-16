### a. How much data does your publisher program send to the message broker in one run?

In one run, the publisher program sends five messages to the message broker. Each message is a serialized UserCreatedEventMessage struct containing two fields: a user_id (string) and a user_name (string). Since each field is a short string and the message uses Borsh serialization (which is binary and compact), the total data size per message is relatively small—likely around a few dozen bytes per message. Thus, across five messages, the total data sent is approximately a few hundred bytes, depending on the length of the strings used.

### b. The URL “amqp://guest:guest@localhost:5672” is the same as in the subscriber program. What does it mean?

The URL amqp://guest:guest@localhost:5672 indicates that both the publisher and subscriber are connecting to the same RabbitMQ message broker using the same access credentials. The first guest is the username, and the second guest is the password for authentication. localhost means that the broker is running on the local machine, and 5672 is the default port used by RabbitMQ for AMQP protocol communication. Using the same URL ensures that both components (publisher and subscriber) are interacting with the same message exchange system, enabling successful message delivery and handling.

### Running RabbitMQ as message broker

![Image](https://github.com/user-attachments/assets/38e2220a-6ab2-47a8-a492-83ae1acff2cd)