#### Overview ####
<ul>
    <li>Why use MQ?</li>
    <li>What are the advantages and disadvantages of using MQ?</li>
    <li>How to ensure that MQ messages are not lost?</li>
    <li>How to ensure the high availability of MQ?</li>
</ul>

#### Why use MQ? ####
I believe everyone has heard such a sentence: a good architecture is not designed, it is evolved.

This sentence is also applicable in the scenario of introducing MQ. The use of MQ must have its own reason and is used to solve practical problems. Instead of seeing others use it, I also use it for a bit.

In fact, there are quite a lot of scenarios using MQ, but there are three cores:
<ul>
    <li>Asynchronous</li>
    <li>Decoupling</li>
    <li>Peaking and Valley Filling</li>
</ul>

#### Asynchronous ####
We show through the actual case: Assume that the A system receives a request, it needs to write the SQL in its own local library, and then needs to call the interface of the three systems of the BCD.

Assume that the local write library is 3ms, and the three systems that call BCD are 300ms, 450ms, and 200ms respectively.

Then the total delay of the final request is 3 + 300 + 450 + 200 = 953ms, which is close to 1s, and the user may feel too slow.

At this point the whole system is probably like this:
![asynchronous](https://user-images.githubusercontent.com/5309726/61592858-2365c280-ac0b-11e9-9ed9-274c6e6c8d39.png)

But once MQ is used, System A only needs to send 3 messages to 3 message queues in MQ, and then returns to the user.

Suppose that it takes 20ms to send a message to MQ, then the user perceives that the interface takes only 20 + 3 = 23ms, and the user has almost no perception.

At this point the entire system structure is probably like this:
![asynchronous-2](https://user-images.githubusercontent.com/5309726/61592898-89eae080-ac0b-11e9-841d-f5d9f65e4080.png)

It can be seen that the performance of the interface can be greatly improved by the asynchronous function of MQ.

#### Decoupling ####

Assume that the A system needs to push the data submitted by the user to the B and C systems at the same time when the user has an operation.

At this time, the buddy in charge of the A system thinks: Nothing, the B and C systems provide me with an Http interface or an RPC interface. I will not push the data in the past. The buddy responsible for the A system is beautiful.

As shown below:

![decoupling](https://user-images.githubusercontent.com/5309726/61592937-efd76800-ac0b-11e9-81ac-e4d05f253189.png)

Everything looks great, but as the business quickly iterates, System D also wants this data. In this case, the development of the A system will change, and add a D when sending data to the BC.

However, the more you find out later, the trouble is coming...

The entire system seems to have more than this data to be sent to the BCD, and the second and third data to be sent to the BCD. Even sometimes joined E, F and other systems, they also want this data.

And sometimes the B system suddenly does not want this data, the A system should be changed, the development of the A system buds scalp numb.

The more complicated scenario is that data is transmitted to other systems through the interface. Sometimes it is necessary to consider some abnormal situations such as retry and timeout. It is really white hair...

Look at the picture below and experience this helpless scene:
![decoupling-2](https://user-images.githubusercontent.com/5309726/61592981-6ffdcd80-ac0c-11e9-90ff-f792591f9711.png)

At this time, it is time for our MQ to appear!

In this case, using MQ to decouple is appropriate, because the buddy in charge of the A system only needs to throw the message to the MQ, and other systems can subscribe to the message as needed.

Even if a system does not need this data, it will not require the A system to change the code.

Look at the following picture of joining MQ decoupling, is it refreshing a lot!
![decoupling-3](https://user-images.githubusercontent.com/5309726/61593006-b4896900-ac0c-11e9-898a-b42da2e38304.png)

#### Peak clipping ####

For example, our order system will write data to the database when placing an order. However, the database can only support concurrent writes of about 1000 per second, and it is easy to crash when the amount of concurrent output is high.

At the peak period, there are more than 100 concurrent, but at the peak, the amount of concurrency will suddenly increase to more than 5000. At this time, the database is definitely dead.

As shown below, to feel the desperation of the database being killed:
![peak-clipping](https://user-images.githubusercontent.com/5309726/61593071-6fb20200-ac0d-11e9-8f5f-3ba6d99495a2.png)

But after using MQ, the situation has changed!

The message is saved by MQ, and then the system can consume according to its own spending power, such as 1000 data per second, so slowly write to the database, so that the database will not be killed:

The whole process is as shown below:

![peak-clipping2](https://user-images.githubusercontent.com/5309726/61593061-45604480-ac0d-11e9-855d-61ef65f55a69.png)

As for why it is called a peak-filling valley? Let’s take a look at this picture:
![peak-clipping3](https://user-images.githubusercontent.com/5309726/61593097-d505f300-ac0d-11e9-9a81-b0084aef3792.png)

If there is no use of MQ, there is a " peak " at the peak of the concurrency, and then a low-concurrency " valley " after the peak period .

But after using MQ, the speed of restricting consumption messages is 1000, but in this case, the data generated during the peak period is bound to be backlogged in MQ, and the peak is "cut".

However, due to the backlog of news, the speed of consumption messages will remain at 1000QPS for a period of time after the peak period, until the news of the backlog is consumed, this is called “filling the valley”.

Through the above analysis, you can know why you want to use MQ, and what are the benefits of using MQ. Know why, understand why your system uses MQ.

In this way, when someone asks you why you want to use MQ, there will be no such thing as "our team leader wants to use MQ."

#### What are the advantages and disadvantages of using MQ? ####
When I saw this problem, I used it! The advantages have been mentioned above, but this shortcoming is awkward. It seems that there are no flaws.

If you think like this, it is a big mistake. In the process of designing the system, besides knowing clearly why you should use this thing, you should also think about the disadvantages after using it. In this way, you can have a bottom in your heart and take precautions.

Next we will discuss, what are the disadvantages of using MQ?

<table>
    <tbody>
        <tr>
            <td>Reduced system availability</td>
            <td>
                Think about it, the above decoupling scenario, the original system A buddy wants to send the system key data to the BC system, and now suddenly joined an MQ, and now the BC system receives data to be received through MQ.
                <br />
                <br />
                But have you considered a question, what if the MQ is hung up? This raises the question. After joining MQ, is the availability of the system reduced?
                <br />
                <br />
                Because there is one more risk factor: MQ may hang. As long as the MQ hangs, the data is gone, and the system is running wrong.
            </td>
        </tr>
        <tr>
            <td>Increased system complexity</td>
            <td>
                Originally my system can be completed by calling the interface, but after adding an MQ, we need to consider the problem of repeated consumption of messages, message loss, and even message order.
                <br />
                <br />
                In order to solve these problems, it is necessary to introduce a lot of complicated mechanisms, so that the complexity of the system is improved.
            </td>
        </tr>
        <tr>
            <td>Data consistency problem</td>
            <td>
                Originally, the A system calls the BC system interface. If the BC system fails, an exception will be thrown and returned to the A system for the A system to know. In this case, the rollback operation can be performed.
                <br />
                <br />
                However, after using MQ, the A system finished sending the message and it was considered successful. And just C system failed to write the database, but A thinks C has succeeded? As a result, the data is inconsistent.
                <br />
                <br />
                After analyzing the advantages and disadvantages of introducing MQ, I understand that there are many advantages to using MQ, but I will find that the disadvantages brought by it will require you to make various additional system designs to make up.
                <br />
                <br />
                In the end, you may find that the whole system is several times more complicated, so you should make trade-offs based on these considerations when designing the system. Many times you will find that it is still used...
            </td>
        </tr>
    </tbody>
</table>

How to ensure that MQ messages are not lost?
After using MQ, you should also be concerned about the loss of messages. Here we pick RabbitMQ to explain it.

#### Producer lost data ####
When the RabbitMQ producer sends the data to the rabbitmq, the data may be lost in the network transmission. At this time, the RabbitMQ does not receive the message and the message is lost.

RabbitMQ provides two ways to solve this problem:

<table>
    <tbody>
        <tr>
            <td>Transaction mode</td>
            <td>
                Open a transaction via `channel.txSelect` before the producer sends the message, then send the message
                <br />
                <br />
                If the message is not successfully received by RabbitMQ, the producer will receive an exception and at this point the transaction can be rolled back to `channel.txRollback` and resent. If RabbitMQ receives this message, it can submit the transaction `channel.txCommit`.
                <br />
                <br />
                But in this way, the throughput and performance of the producer will be much lower, and now generally do not do this.
            </td>
        </tr>
        <tr>
            <td>Confirm mechanism</td>
            <td>
                This confirm mode is set at the producer, that is, each time a message is written, a unique id is assigned, and then RabbitMQ will return an ack after receiving it, telling the producer that the message is ok.
                <br />
                <br />
                If rabbitmq does not process this message, then call back a nack interface, at which point the producer can resend.
                <br />
                <br />
                The <strong>biggest difference</strong> between the transaction mechanism and the confirm mechanism is that the transaction mechanism is synchronous, and it will block there after committing a transaction.
                <br />
                <br />
                But the confirm mechanism is asynchronous. After sending a message, you can send the next message. Then after the message rabbitmq receives it, it will asynchronously call back an interface to inform you that the message has been received.
            </td>
        </tr>
    </tbody>
</table>


Therefore, in general, in the <strong>producer to avoid data loss, the use of the confirm mechanism</strong>.

#### Rabbitmq lost the data ####
The RabbitMQ cluster will also lose the message. This problem is also mentioned in the official documentation tutorial. That is to say, after the message is sent to RabbitMQ, the default is no landing disk. In case the RabbitMQ is down, the message is lost. .

So in order to solve this problem, RabbitMQ provides a persistence mechanism that persists to disk after the message is written.

This way, even if it is down, the previously stored data will be automatically restored after recovery. This mechanism ensures that the message will not be lost.

There are two steps to setting up persistence:
<ul>
    <li>The first one is to set the queue to be persistent when creating the queue, so that rabbitmq can guarantee the metadata of the queue, but it will not persist the data in the queue.</li>
    <li>The second is to set the deliveryMode of the message to 2 when sending the message, which is to set the message to be persistent. At this time, rabbitmq will persist the message to the disk.</li>
</ul>

But in this case, someone might say: If the message is sent to RabbitMQ, it will hang up if it has not been persisted to the disk, and the data is lost. What should I do?

For this problem, it is actually guaranteed together with the above confirm mechanism, that is, the ack message is sent to the producer after the message is persisted to the disk.

In the unlikely event that the extreme situation is encountered, the producer can perceive that the producer can retry sending messages to other RabbitMQ nodes.

#### Consumer lost data ####

RabbitMQ consumer lost the data like this: When you consume the message, you just got the message, and the result process hangs. At this time, RabbitMQ will think that you have consumed the product successfully, and this data is lost.

For this problem, we must first explain the mechanism of RabbitMQ consumption message: when the consumer receives the message, it will send an ack to RabbitMQ, telling RabbitMQ that the message is consumed, so RabbitMQ will delete the message.

However, by default, the operation of sending ack is automatically submitted, that is, the consumer will automatically return ack to RabbitMQ upon receiving the message, so there will be a problem of losing the message.

So the solution to this problem is to turn off the automatic submission ack of the RabbitMQ consumer and manually submit the ack after the consumer has processed the message.

So even if you encounter the above situation, RabbitMQ will not delete this message, it will re-issue this message after your program restarts.

#### How to ensure the high availability of MQ? ####
After using MQ, we definitely hope that MQ has high availability features, because it is impossible to accept the machine down, it is impossible to send and receive messages.

This piece is also based on the classic MQ of RabbitMQ to illustrate:

RabbitMQ is more representative, because it is based on master-slave high availability, we use him as an example to explain how the first MQ high availability is achieved.

Rabbitmq has three modes:
<ul>
    <li>Stand-alone mode</li>
    <li>Normal cluster mode</li>
    <li>Mirrored cluster mode</li>
</ul>

<table>
    <tbody>
        <tr>
            <td>Stand-alone mode</td>
            <td>
                Stand-alone mode is demo level, which means that only one machine deploys a RabbitMQ program.
                <br />
                <br />
                There will be a single point problem, and the downtime will be finished, there is no high availability. Generally, you have started to play locally, and no one uses the stand-alone mode.
            </td>
        </tr>
        <tr>
            <td>Ordinary cluster mode</td>
            <td>
                This mode means to start multiple rabbitmq instances on multiple machines. Similar to the master-slave mode.
                <br />
                <br />
                However, the created queue will only be placed on one master rabbtimq instance, and the other instances will synchronize the RabbitMQ metadata that receives the message.
                <br />
                <br />
                When you consume a message, if the RabbitMQ instance you are connecting to is not an instance of the Queue data, RabbitMQ will pull the data from the instance that stores the Queue data and return it to the client.
                <br />
                <br />
                In general, this method is a bit cumbersome, not really distributed, every time the consumer connects to an instance and pulls the data, if connected to an instance that does not store the queue data, this will cause additional performance overhead. If you pull from an instance of the Queue, it will cause a single instance performance bottleneck.
                <br />
                <br />
                If the instance of the queue is down, it will cause other instances to fail to pull data. This cluster cannot consume the message and does not achieve true high availability.
                <br />
                <br />
                So this thing is awkward, there is no such thing as high availability. This solution is mainly to improve the throughput, that is, let multiple nodes in the cluster serve the read and write operations of a queue.
            </td>
        </tr>
        <tr>
            <td>Mirror cluster mode</td>
            <td>
                The mirrored cluster mode is the true high availability mode of rabbitmq. It is different from the normal cluster mode: the created queue will exist on multiple instances regardless of the metadata or the message in the queue.
                <br />
                <br />
                Each time a message is written to the queue, the message is automatically synchronized to the queue of multiple instances.
                <br />
                <br />
                In this case, any other machine can be used to provide services, so that it is truly high availability.
                <br />
                <br />
                But there are also some bad things:
                <ul>
                    <li>
                        Performance overhead is too high, messages need to synchronize all machines, resulting in network bandwidth pressure and heavy consumption
                    </li>
                    <li>Low scalability: unable to solve the problem of a large amount of queue data, resulting in the queue can not be linearly expanded.</li>
                </ul>
                Even if a machine is added, that machine will contain all the data of the queue, and the queue data is not distributed.
                <br />
                <br />
                For the high availability of RabbitMQ, the general practice of turning on the mirror cluster mode is to achieve high availability at least, one node is down, and other nodes can continue to provide services.
            </td>
        </tr>
    </tbody>
</table>
