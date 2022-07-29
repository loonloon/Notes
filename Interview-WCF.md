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
            <td></td>
        </tr>
        <tr>
            <td>Data Contract</td>
            <td></td>
        </tr>
        <tr>
            <td>Message Contract</td>
            <td></td>
        </tr>
        <tr>
            <td>Fault Contract</td>
            <td></td>
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


