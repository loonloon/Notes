#### Episode 1 ####
![rabbitMQ](https://user-images.githubusercontent.com/5309726/61098086-516f3800-a490-11e9-94a5-ed25c194c258.png)

#### Episode 2 ####
* Management Plugin
  * `rabbitmq-plugins enable rabbitmq_management`
  * localhost:15672/
* Basic Commands
<table>
  <tr>
    <th>Command</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>rabbitmqctl stop_app</td>
    <td>Stops the RabbitMQ application</td>
  </tr>
  <tr>
    <td>rabbitmqctl start_app</td>
    <td>Starts the RabbitMQ application</td>
  </tr>
  <tr>
    <td>rabbitmqctl reset</td>
    <td>Returns a RabbitMQ node to its virgin state, <strong>the RabbitMQ application must have been stopped</strong></td>
  </tr>
  <tr>
    <td>rabbitmqctl add_user</td>
    <td>E.g. rabbitmqctl add_user janeway changeit</td>
  </tr>
  <tr>
    <td>rabbitmqctl set_user_tags</td>
    <td>E.g. rabbitmqctl set_user_tags janeway administrator</td>
  </tr>
  <tr>
    <td>rabbitmqctl set_permissions</td>
    <td>E.g. rabbitmqctl set_permissions -p / janeway ".*" ".*" ".*"</td>
  </tr>
</table>
