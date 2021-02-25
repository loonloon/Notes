#### Section 1 ####
##### Section 1.2 .NET Core History - Performance #####

![image](https://user-images.githubusercontent.com/5309726/107108457-e73c9600-6872-11eb-8c41-905d86558a65.png)

##### Section 1.3 .NET Core Concepts and Definitions #####
* Why .NET Standard?
  * Code sharing
* .NET Standard 2.0 doubles the APIs than 1.0
  * Deployment Options
    * Self Contained
    * Shared Framework

---

#### Section 2 ####
##### Section 2.1 Measuring CPU #####
* Types of profiles
  * Sampling
  * Instrumentation

<table>
  <tr>
      <th>Sampling</th>
  </tr>
  <tr>
      <td>Huge number of snapshots with callstacks (Aggrerates this data)</td>
  </tr>
  <tr>
    <td>Can be done on a running process without restart</td>
  </tr>
  <tr>
    <td>Minimal overhead</td>
  </tr>
  <tr>
    <td>Non CPU work is not visible (E.g. Thread.Sleep(), I/O operation)</td>
  </tr>
</table>
 
![image](https://user-images.githubusercontent.com/5309726/109120781-59710e00-7781-11eb-86f1-0ed294b4fb40.png)

![image](https://user-images.githubusercontent.com/5309726/109122806-1b291e00-7784-11eb-84f7-7373323a2fc2.png)

<table>
    <tr>
        <th>Instrumentation</th>
    </tr>
    <tr>
        <td>Injects code to measure methods</td>
    </tr>
    <tr>
      <td>Measures also non CPU work</td>
    </tr>
    <tr>
      <td>Can pontentiallu have more overhead</td>
    </tr>
    <tr>
      <td>No attach / detach</td>
    </tr>
</table>

![image](https://user-images.githubusercontent.com/5309726/109122903-385dec80-7784-11eb-9896-961da3218b66.png)

##### Section 2.2 #####


---
