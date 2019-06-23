#### Episode 1 ####
* Server Side Templating vs Single Page App
![webpack-01](https://user-images.githubusercontent.com/5309726/59971196-1a50e980-95aa-11e9-9503-6bc97f626b33.png)

* Approach comparison

![webpack-02](https://user-images.githubusercontent.com/5309726/59971223-829fcb00-95aa-11e9-8ff5-f325a221bde2.png)


* Amount of JS Code

![webpack-03](https://user-images.githubusercontent.com/5309726/59971264-1e313b80-95ab-11e9-9bad-66446a794c56.png)

#### Episode 2 ####
* Javascript Modules
  * Splitting a single javascript code base to many separate javascript files in the application
* What problems start to arise with separating our code into separate files?
  * Concern about the order in which our code is executed
  * Concern about the performance over loading many files up over an HTTP connection
* The purpose of webpack
  * Take big collection of tiny little javascript modules and merge them all into one big javascript file
  * Also ensuring that each module is executed in the correct order
  
![webpack-core](https://user-images.githubusercontent.com/5309726/59971351-05298a00-95ad-11e9-8597-50527c475450.png)

* Different module system
  * to link different files together and share some code between them

![module-system](https://user-images.githubusercontent.com/5309726/59971447-e6c48e00-95ae-11e9-99a3-10170f17e114.png)

* Webpack configuration (2 required properties)
  * Enrty property
  * Output property
    * path module from node.js
    * __dirname
  
![webpack-config](https://user-images.githubusercontent.com/5309726/59977602-a3493e80-9605-11e9-99e3-8b1848f577fb.png)

#### Episode 3 ####
