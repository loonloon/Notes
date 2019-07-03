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
![babel-loader](https://user-images.githubusercontent.com/5309726/60185167-07654000-985c-11e9-9c0e-122a4c351c6d.png)

![apply-babel](https://user-images.githubusercontent.com/5309726/60185498-9c683900-985c-11e9-8b92-216a17bdc37e.png)

![common-es2015](https://user-images.githubusercontent.com/5309726/60186954-5660a480-985f-11e9-923f-623aa9abca58.png)

![css-loader](https://user-images.githubusercontent.com/5309726/60187607-747ad480-9860-11e9-82b2-6a4c594b46fc.png)

* Use `extract-text-webpack-plugin (DEPRECATED)` to extracts text from a bundle into a separate file
  * Latest `mini-css-extract-plugin`
  
![image-loader](https://user-images.githubusercontent.com/5309726/60190013-83638600-9864-11e9-88ac-acce1f66d370.png)
* Required loaders
  * npm install --save-dev imagemin-svgo
  * npm install file-loader --save-dev
 * Define `publicPath` in `output` object, URL loader can use it and prepended to the image URL.

#### Episode 4 ####
* Code splitting

![client-server](https://user-images.githubusercontent.com/5309726/60270030-229a8300-9922-11e9-9745-d7754ed50cc9.png)

* Webpack is looking for `System.import` call, and if it sees a call to `System.import`, it's going to add additional code.

#### Episode 5 ####
* Vendor caching

![code-splitting](https://user-images.githubusercontent.com/5309726/60273073-df431300-9927-11e9-99e9-c78e77091b5b.png)
* `CommonsChunkPlugin` E.g. any files that importing react library, it should point to `vendor.js`
* `HtmlWebpackPlugin`, automatically find all the generated scripts and add to `index.html` file
* `rimraf`, clean up folder for every re-build

#### Episode 6 ####
