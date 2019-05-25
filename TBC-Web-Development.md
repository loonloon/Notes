#### What happens when you type in a URL ####
* https://wsvincent.com/what-happens-when-url/
1.  You enter a URL into a web browser
2. The browser looks up the IP address for the domain name via DNS
3. The browser sends a HTTP request to the server
4. The server sends back a HTTP response
5. The browser begins rendering the HTML
6. The browser sends requests for additional objects embedded in HTML (images, css, JavaScript) and repeats steps 3-5.
7. Once the page is loaded, the browser sends further async requests as needed.

#### Web architecture ####
* https://engineering.videoblocks.com/web-architecture-101-a3224e126947

#### SQL VS NoSQL ####
* https://community.hortonworks.com/questions/88325/what-is-the-difference-between-sql-and-no-sql-whic.html
* https://blog.panoply.io/sql-or-nosql-that-is-the-question

#### What is Microservices? ####
* https://microservices.io/

#### What is REST api? ####
* https://www.quora.com/What-are-REST-APIs-What-does-rest-mean-How-does-it-work

#### 3 Layer vs 3 Tier Architecture ####
<table>
  <tr>
    <th>3 Layer</th>
    <th>3 Tier</th>
  </tr>
  <tr>
    <td>Logical structuring</td>
    <td>Physical structuring</td>
  </tr>
  <tr>
    <td>The User Interface Layer (UIL), Business Logic Layer (BLL) and Database Access Layer (DAL) resides as 3 different project and the output of these 3 projects (.dll file) must be together in the same server or on same machine in order for the system to run</td>
    <td>The User Interface Layer (UIL), Business Logic Layer (BLL) and Database Access Layer (DAL) reside as 3 different projects. But each of the projects can be deployed at the different server or at the different machines and distributed functionality is explored</td>
  </tr>
  <tr>
    <td>Improve readability and reusability</td>
    <td>Scalability</td>
  </tr>
</table>

#### SQL Procedural vs Set-Based ####
* https://www.itprotoday.com/analytics-and-reporting/programming-sql-set-based-way
<table>
  <tr>
    <th>Procedural</th>
    <th>Set-Based</th>
  </tr>
  <tr>
    <td>Tell the system "what to do" along with "how to do" it.</td>
    <td>Lets you specify "what to do", but does not let you specify "how to do"</td>
  </tr>
</table>
