#### Difference between WCF and Web service ####

<table>
    <tbody>
        <tr>
            <th>Features</th>
            <th>Web Service</th>
            <th>WCF</th>
        </tr>
        <tr>
            <td>Hosting</td>
            <td>It can be hosted in IIS</td>
            <td>
                <ul>
                    <li>IIS</li>
                    <li>Windows Activation Service</li>
                    <li>Self Hosting</li>
                    <li>Windows Service</li>
                </ul>
            </td>
        </tr>
        <tr>
            <td>Programming</td>
            <td>[WebService] attribute has to be added to the class</td>
            <td>[ServiceContract] attribute has to be added to the class</td>
        </tr>
        <tr>
            <td>Model</td>
            <td>[WebMethod] attribute represents the method exposed to client</td>
            <td>[OperationContract] attribute represents the method exposed to client</td>
        </tr>
        <tr>
            <td>Operation</td>
            <td>
                <ul>
                    <li>One-way</li>
                    <li>Request - Response</li>
                </ul>
            </td>
            <td>
                <ul>
                    <li>One-way</li>
                    <li>Request - Response</li>
                    <li>Duplex</li>
                </ul>
            </td>
        </tr>
        <tr>
            <td>XML</td>
            <td>System.Xml.Serialization namespace is used for serialization</td>
            <td>System.Runtime.Serialization namespace is used for serialization</td>
        </tr>
        <tr>
            <td>Encoding</td>
            <td>
                <ul>
                    <li>XML 1.0</li>
                    <li>MTOM (Message Transmission Optimization Mechanism)</li>
                    <li>DIME</li>
                    <li>Custom</li>
                </ul>
            </td>
            <td>
                <ul>
                    <li>XML 1.0</li>
                    <li>MTOM (Message Transmission Optimization Mechanism)</li>
                    <li>Binary</li>
                    <li>Custom</li>
                </ul>
            </td>
        </tr>
        <tr>
            <td>Transports</td>
            <td>Can be accessed through HTTP, TCP, Custom</td>
            <td>Can be accessed through HTTP, TCP, Named pipes, MSMQ, P2P, Custom</td>
        </tr>
        <tr>
            <td>Protocols</td>
            <td>Security</td>
            <td>
                <ul>
                    <li>Security</li>
                    <li>Reliable Messaging</li>
                    <li>Transactions</li>
                </ul>
            </td>
        </tr>
    </tbody>
</table>

#### EndPoint ####
WCF Service is a program that exposes a collection of Endpoints. All the WCF communications are take place through end point. <b>End point consists of 3 components</b>.

##### Address #####
Basically URL, specifies where this WCF service is hosted. Client will use this url to connect to the service. e.g, http://localhost:8090/MyService/SimpleCalculator.svc

##### Binding #####
Binding will describes how client will communicate with service. There are different protocols available for the WCF to communicate to the Client.

<table>
    <tbody>
        <tr>
            <th></th>
            <th>Description</th>
        </tr>
        <tr>
            <td>Transport</td>
            <td>Defines the base protocol to be used like HTTP, Named Pipes, TCP, and MSMQ are some type of protocols.</td>
        </tr>
        <tr>
            <td>Encoding (Optional)</td>
            <td>hree types of encoding are available-Text, Binary, or Message Transmission Optimization Mechanism (MTOM). MTOM is an interoperable message format that allows the effective transmission of attachments or large messages (greater than 64K).</td>
        </tr>
        <tr>
            <td>Protocol (Optional)</td>
            <td>Defines information to be used in the binding such as Security, transaction or reliable messaging capability.</td>
        </tr>
    </tbody>
</table>

The following table gives some list of protocols supported by WCF binding.

<table>
    <tbody>
        <tr>
            <th>Binding</th>
            <th>Description</th>
        </tr>
        <tr>
            <td>BasicHttpBinding</td>
            <td>Basic Web service communication. No security by default</td>
        </tr>
        <tr>
            <td>WSHttpBinding</td>
            <td>Web services with WS-* support. Supports transactions</td>
        </tr>
        <tr>
            <td>WSDualHttpBinding</td>
            <td>Web services with duplex contract and transaction support</td>
        </tr>
        <tr>
            <td>WSFederationHttpBinding</td>
            <td>Web services with federated security. Supports transactions</td>
        </tr>
        <tr>
            <td>MsmqIntegrationBinding</td>
            <td>Communication directly with MSMQ applications. Supports transactions</td>
        </tr>
        <tr>
            <td>NetMsmqBinding</td>
            <td>Communication between WCF applications by using queuing. Supports transactions</td>
        </tr>
        <tr>
            <td>NetNamedPipeBinding</td>
            <td>Communication between WCF applications on same computer. Supports duplex contracts and transactions</td>
        </tr>
        <tr>
            <td>NetPeerTcpBinding</td>
            <td>Communication between computers across peer-to-peer services. Supports duplex contracts</td>
        </tr>
        <tr>
            <td>NetTcpBinding</td>
            <td>Communication between WCF applications across computers. Supports duplex contracts and transactions</td>
        </tr>
    </tbody>
</table>

![image](https://user-images.githubusercontent.com/5309726/181728564-78f5c69d-692e-404e-8b6e-979a89f36bee.png)

##### Contract #####
Collection of operation that specifies what the endpoint will communicate with outside world. Usually name of the Interface will be mentioned in the Contract, so the client application will be aware of the operations which are exposed to the client.

![image](https://user-images.githubusercontent.com/5309726/181728220-b5a41954-260b-4a08-9f3c-729b67d7f3b0.png)

##### WCF Architecture #####

![image](https://user-images.githubusercontent.com/5309726/181730122-867b6e07-45b7-464a-8229-5e88c5ea1d0b.png)

#### Contracts ####
In WCF, all services are exposed as contracts. Contract is a platform-neutral and standard way of describing what the service does. Mainly there are <b>4 types of contracts</b> available in WCF.

<table>
    <tbody>
        <tr>
            <th>Contract</th>
            <th>Description</th>
        </tr>
        <tr>
            <td>Service Contract</td>
            <td>Service contract describes the operation that service provide</td>
        </tr>
        <tr>
            <td>Data Contract</td>
            <td>A data contract is a formal agreement between a service and a client that abstractly describes the data to be exchanged</td>
        </tr>
        <tr>
            <td>Message Contract</td>
            <td>Message is the packet of data which contains important information. WCF uses these messages to transfer information from Source to destination</td>
        </tr>
        <tr>
            <td>Fault Contract</td>
            <td>WCF provides the option to handle and convey the error message to client from service using SOAP Fault contract.</td>
        </tr>
    </tbody>
</table>

#### Service Runtime ####
It contains the behaviors that occur during runtime of service.

<table>
    <tbody>
        <tr>
            <th>Behaviour</th>
            <th>Description</th>
        </tr>
        <tr>
            <td>Throttling</td>
            <td>Controls how many messages are processed</td>
        </tr>
        <tr>
            <td>Error</td>
            <td>Specifies what occurs, when internal error occurs on the service</td>
        </tr>
        <tr>
            <td>Metadata</td>
            <td>Tells how and whether metadata is available to outside world</td>
        </tr>
        <tr>
            <td>Instance</td>
            <td>Specifies how many instance of the service has to be created while running</td>
        </tr>
        <tr>
            <td>Transaction</td>
            <td>Enables the rollback of transacted operations if a failure occurs</td>
        </tr>
        <tr>
            <td>Dispatch</td>
            <td>Controls how a message is processed by the WCF Infrastructure</td>
        </tr>
    </tbody>
</table>

#### Channels ####
Channels are the core abstraction for sending message to and receiving message from an Endpoint. Broadly we can categories channels as

<table>
    <tbody>
        <tr>
            <th>Channel</th>
            <th>Description</th>
        </tr>
        <tr>
            <td>Transport</td>
            <td>Handles sending and receiving message from network. Protocols like HTTP, TCP name pipes and MSMQ.</td>
        </tr>
        <tr>
            <td>Protocol</td>
            <td>Implements SOAP based protocol by processing and possibly modifying message. e.g. WS-Security and WS-Reliability.</td>
        </tr>
    </tbody>
</table>

#### Bindings and Channel Stacks ####
In WCF all the communication details are handled by channel, it is a stack of channel components that all messages pass through during runtime processing. The bottom-most component is the transport channel. This implements the given transport protocol and reads incoming messages off the wire. 

The transport channel uses a message encoder to read the incoming bytes into a logical Message object for further processing.

![image](https://user-images.githubusercontent.com/5309726/184315744-27e7bc9c-77be-400c-a913-6b6d110e82c6.png)

After that, the message bubbles up through the rest of the channel stack, giving each protocol channel an opportunity to do its processing, until it eventually reaches the top and WCF dispatches the final message to your service implementation. Messages undergo significant transformation along the way.

So WCF provides easy way of achieving this using end point. In end point we will specify address, binding and contract. To know more about end point.

#### Metadata Exchange ####
WCF services use metadata to describe how to interact with the service's endpoints so that tools, such as Svcutil.exe, can automatically generate client code for accessing the service

#### Instance Management ####

<table>
    <tbody>
        <tr>
            <th>Service</th>
            <th>Description</th>
            <th</th>
        </tr>
        <tr>
            <td>Per-Call</td>
            <td>Service instance will be created for each client request. This Service instance will be disposed after response is sent back to client. 
</td>
            <td>
                <img src="https://user-images.githubusercontent.com/5309726/184319202-8e03c3d0-8a8d-435f-8103-2f631f3001d2.png" />
            </td>
        </tr>
        <tr>
            <td>Per-Session</td>
            <td>Logical session between client and service will be maintained. When the client creates new proxy to particular service instance, a dedicated service instance will be provided to the client. It is independent of all other instance.</td>
            <td>
                <img src="https://user-images.githubusercontent.com/5309726/184320086-c04298dd-5690-4ecb-931f-b9b10ad10f22.png" />
            </td>
        </tr>
        <tr>
            <td>Singleton</td>
            <td>All clients are independently connected to the same single instance. This singleton instance will be created when service is hosted and, it is disposed when host shuts down.</td>
            <td>
                <img src="https://user-images.githubusercontent.com/5309726/184320380-456da500-4cc9-4c1b-baeb-d53c7c226c58.png" />
            </td>
        </tr>
    </tbody>
</table>

#### Instance Deactivation ####

<table>
    <tbody>
        <tr>
            <th>RealeaseInstanceMode</th>
            <th>Description</th>
            <th</th>
        </tr>
        <tr>
            <td>RealeaseInstanceMode.None</td>
            <td>This property means that it will not affect the instance lifetime. By default ReleaseInstanceMode property is set to 'None'</td>
            <td>
                <img src="https://user-images.githubusercontent.com/5309726/184324310-f4d10229-5645-48a3-a2c4-ef35285bcf56.png" />
            </td>
        </tr>
        <tr>
            <td>RealeaseInstanceMode.BeforeCall</td>
            <td>
                This property means that it will create new instance before a call is made to the operation. <br /><br /> If the instance is already exist,WCF deactivates the instance and calls Dispose() before the call is done. This is designed to optimize a method such as Create()
            </td>
            <td>
                <img src="https://user-images.githubusercontent.com/5309726/184324378-670007b5-95d5-4d73-bfcc-0b070f6bad3f.png" />
            </td>
        </tr>
        <tr>
            <td>RealeaseInstanceMode.AfterCall</td>
            <td>This property means that it will deactivate the instance after call is made to the method. <br /><br /> This is designed to optimize a method such a Cleanup()</td>
            <td>
                <img src="https://user-images.githubusercontent.com/5309726/184324532-a95703db-a9d2-4ffa-96f5-97097ec5c032.png" />
            </td>
        </tr>
        <tr>
            <td>RealeaseInstanceMode.BeforeAndAfterCall</td>
            <td>This is means that it will create new instance of object before a call and deactivates the instance after call. This has combined effect of using ReleaseInstanceMode.BeforeCall and ReleaseInstanceMode.AfterCall</td>
            <td>
                <img src="https://user-images.githubusercontent.com/5309726/184324600-31c514e9-df07-4f35-9007-f54a0d2b19f4.png" />
            </td>
        </tr>
    </tbody>
</table>

#### Throttling ####
WCF throttling provides some properties that you can use to limit how many instances or sessions are created at the application level. Performance of the WCF service can be improved by creating proper instance.

<table>
    <tbody>
        <tr>
            <th>Attribute</th>
            <th>Description</th>
        </tr>
        <tr>
            <td>maxConcurrentCalls</td>
            <td>Limits the total number of calls that can currently be in progress across all service instances. The default is 16.</td>
        </tr>
        <tr>
            <td>maxConcurrentInstances</td>
            <td>The number of InstanceContext objects that execute at one time across a ServiceHost. The default is Int32.MaxValue.</td>
        </tr>
        <tr>
            <td>maxConcurrentSessions</td>
            <td>A positive integer that limits the number of sessions a ServiceHost object can accept. The default is 10.</td>
        </tr>
    </tbody>
</table>

#### Operations ####
