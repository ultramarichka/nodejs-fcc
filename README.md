https://github.com/maxogden/art-of-node#callbacks

1. HELLO WORLD

   To make a Node.js program, create a new file with a .js extension and  
  start writing JavaScript! Execute your program by running it with the node  
  command. e.g.:  
   
    $ node program.js  
   
  You can write to the console in the same way as in the browser:  
   
     console.log("text")  
   
2. Write a program that accepts one or more numbers as command-line arguments  
  and prints the sum of those numbers to the console (stdout). 

  You can access command-line arguments via the global process object. The  
  process object has an argv property which is an array containing the  
  complete command-line. i.e. process.argv.  
   
  To get started, write a program that simply contains:  
   
     console.log(process.argv)  
   
  Run it with node program.js and some numbers as arguments. e.g:  
   
     $ node program.js 1 2 3  
   
  In which case the output would be an array looking something like:  
   
     [ 'node', '/path/to/your/program.js', '1', '2', '3' ]  
<blank>

    var numbers = process.argv.slice(2);
    var result = numbers.reduce(function(accumulator, currentValue) {
        return Number(accumulator) + Number(currentValue);
    });
    console.log(result);

    /*official solution
    var result = 0;
    for (var i = 2; i < process.argv.length; i++) {
      result += Number(process.argv[i])
    }
    
    console.log(result);*/


3. Write a program that uses a single synchronous filesystem operation to  
  read a file and print the number of newlines (\n) it contains to the  
  console (stdout), similar to running cat file | wc -l.  
   
  The full path to the file to read will be provided as the first  
  command-line argument (i.e., process.argv[2]). You do not need to make  
  your own test file. 

  Documentation on the fs module can be found by pointing your browser here:  
  file:///home/ubuntu/.nvm/versions/node/v6.11.3/lib/node_modules/learnyouno  
  de/node_apidoc/fs.html 

  Buffer objects are Node's way of efficiently representing arbitrary arrays  
  of data, whether it be ascii, binary or some other format.

   Documentation on Buffers can be found by pointing your browser here:  
  file:///home/ubuntu/.nvm/versions/node/v6.11.3/lib/node_modules/learnyouno  
  de/node_apidoc/buffer.html



    var fs = require('fs');  //enable module  with wich my project3.js can read a file
    var buf = fs.readFileSync(process.argv[2]); //returns a Buffer object containing the complete contents of the file.  
    var str = buf.toString();
    var result = str.match(/\n+/g);
    console.log(result.length);


4. Asyncronous I/O


```javascript
var fs = require('fs');  //enable module  with wich my project3.js can read a file
fs.readFile(process.argv[2],'utf8', callback);
function callback(err, data){
  if (err){
    console.log("error");
  } else {
    var result = data.match(/\n+/g);
    console.log(result.length);
  }
}
```

## 5. FILTER DIRECTORY FOR A FILE NAME

 
  Create a program that prints a list of files in a given directory,  
  filtered by the extension of the files. You will be provided a directory  
  name as the first argument to your program (e.g. '/path/to/dir/') and a  
  file extension to filter by as the second argument.  
    ```javascript
    var fs = require('fs');  //enable module  with wich my project3.js can read a file
    //console.log(process.argv);

    fs.readdir(process.argv[2], process.argv[3], callback);
    //console.log('file_extension', process.argv[3]);
    function callback(err, files){
      if (err){
        console.log("error");
      } else {
        var arr = files.filter(returnsTrueIfExtensionMatchs);
        console.log(arr.join('\n'));
      }
    }

    function returnsTrueIfExtensionMatchs(el){
      if (el.match('.'+process.argv[3]+ '$')){
        return true;
      } else {return false;}
    }
    ```

## 6. MAKE IT MODULAR 

  Create a program that prints a list of files in a given directory,  
  filtered by the extension of the files. The first argument is the  
  directory name and the second argument is the extension filter. Print the  
  list of files (one file per line) to the console. You must use  
  asynchronous I/O.  
   
  You must write a module file to do most of the work. The module must  
  export a single function that takes three arguments: the directory name,  
  the filename extension string and a callback function, in that order. The  
  filename extension argument must be the same as what was passed to your  
  program.

  These four things are the contract that your module must follow.  
   
   1. Export a single function that takes exactly the arguments described.  
   2. Call the callback exactly once with an error or some data as described.  
   3. Don't change anything else, like global variables or stdout.  
   4. Handle all the errors that may occur and pass them to the callback.  

    
```javascript
(func6.js)
var fs = require('fs');  //enable module  with wich my project3.js can read a file

module.exports = function (dirPath, extension, callback) {
  function returnsTrueIfExtensionMatchs(el){
    if (el.match('.'+ extension + '$')){
      return true;
    } else {return false;}
  }
  
  fs.readdir(dirPath, extension, ljubajaFunkcija);
  
  function ljubajaFunkcija(err, data){
    if (err){
      callback(err);
    } else {
      var arr = data.filter(returnsTrueIfExtensionMatchs);
      callback(null, arr);
    }
  }  
};
```

<blank>

```javascript
(app6.js)
var mymodule = require('./func6.js'); 

function printFromArr(err, arr){
  if (err){
    console.log("error");
  } else {
    console.log(arr.join('\n'));
  }
}

mymodule(process.argv[2], process.argv[3], printFromArr);
```

## 7.HTTP CLIENT: GET

  Write a program that performs an HTTP GET request to a URL provided to you  
  as the first command-line argument. Write the String contents of each  
  "data" event from the response to a new line on the console (stdout).  

```javascript
var http = require('http');
var url = process.argv[2];
http.get(url, callback);

function callback (response) { 
    response.setEncoding('utf8');
    response.on("error", (err) => {if (err){console.log(err.message);}});
    response.on("data", function (data) {
        console.log(data);
    });
} 

//official solution
/*
 var http = require('http')
    
    http.get(process.argv[2], function (response) {
      response.setEncoding('utf8')
      response.on('data', console.log)
      response.on('error', console.error)
    }).on('error', console.error)

*/
```
## 8. HTTP COLLECT (GET: Collect ALL data from the server)

  Write a program that performs an HTTP GET request to a URL provided to you  
  as the first command-line argument. Collect all data from the server (not  
  just the first "data" event) and then write two lines to the console  
  (stdout).  
   
  The first line you write should just be an integer representing the number  
  of characters received from the server. The second line should contain the  
  complete String of characters sent by the server.  

```javascript
var http = require('http');
var url = process.argv[2];
http.get(url, callback);


function callback (response) { 
    var result ="";
    response.setEncoding('utf8');
    response.on("error", console.error);
    response.on("data", function (data) {
        
        result += data;
        
    });
    response.on('end', function () {
              console.log(result.length);
              console.log(result);
        });
} 
```

  2nd method) Use a third-party package to abstract the difficulties involved in  
  collecting an entire stream of data. Two different packages provide a  
  useful API for solving this problem (there are likely more!): bl (Buffer  
  List) and concat-stream; take your pick!  
   
  <https://npmjs.com/bl> <https://npmjs.com/concat-stream>  

    $ npm install bl  
<blank>

```javascript
var bl = require('bl')  
```

  Both bl and concat-stream can have a stream piped in to them and they will  
  collect the data for you. Once the stream has ended, a callback will be  
  fired with the data:  
   
```javascript
response.pipe(bl(function (err, data) { /* ... */ }))  
// or  
response.pipe(concatStream(function (data) { /* ... */ }))  
```
 
  Note that you will probably need to data.toString() to convert from a  
  Buffer.    

## 9.JUGGLING ASYNC (ASYNC COLLECT)

  This problem is the same as the previous problem (HTTP COLLECT) in that  
  you need to use http.get(). However, this time you will be provided with  
  three URLs as the first three command-line arguments.  
   
  You must collect the complete content provided to you by each of the URLs  
  and print it to the console (stdout). You don't need to print out the  
  length, just the data as a String; one line per URL. The catch is that you  
  must print them out in the same order as the URLs are provided to you as  
  command-line arguments.  

  Counting callbacks is one of the fundamental ways of managing async in  
  Node. Rather than doing it yourself, you may find it more convenient to  
  rely on a third-party library such as [async](https://npmjs.com/async) or  
  [after](https://npmjs.com/after). But for this exercise, try and do it  
  without any external helper library. 

```javascript
var http = require('http');
var url1 = process.argv[2];
var url2 = process.argv[3];
var url3 = process.argv[4];
http.get(url1, callback);


function callback (response) { 
    var result ="";
    response.setEncoding('utf8');
    response.on("error", console.error);
    response.on("data", function (data) {
        
        result += data;
        
    });
    response.on('end', function () {
              
              console.log(result);
    });
    http.get(url2, callback2);    
} 

function callback2 (response) { 
    var result ="";
    response.setEncoding('utf8');
    response.on("error", console.error);
    response.on("data", function (data) {
        
        result += data;
        
    });
    response.on('end', function () {
              
              console.log(result);
    });
    http.get(url3, callback3);    
} 

function callback3 (response) { 
    var result ="";
    response.setEncoding('utf8');
    response.on("error", console.error);
    response.on("data", function (data) {
        
        result += data;
        
    });
    response.on('end', function () {
              
              console.log(result);
    });
} 
```

## 10.TIME SERVER

  Your server should listen to TCP connections on the port provided by the  
  first argument to your program. For each connection you must write the  
  current date & 24 hour time in the format:  
   
     "YYYY-MM-DD hh:mm"  
   
  followed by a newline character. Month, day, hour and minute must be  
  zero-filled to 2 integers. For example:  
   
     "2013-07-06 17:42"  
   
  After sending the string, close the connection.

  # HINTS  
   
  For this exercise we'll be creating a raw TCP server. There's no HTTP  
  involved here so we need to use the net module from Node core which has  
  all the basic networking functions.  
   
  The net module has a method named net.createServer() that takes a  
  function. The function that you need to pass to net.createServer() is a  
  connection listener that is called more than once. Every connection  
  received by your server triggers another call to the listener. The  
  listener function has the signature:  
   
     function listener(socket) { /* ... */ }  
   
  net.createServer() also returns an instance of your server. You must call  
  server.listen(portNumber) to start listening on a particular port.   

  Or, if you want to be adventurous, use the strftime package from npm. The  
  strftime(fmt, date) function takes date formats just like the unix date  
  command. You can read more about strftime at:  
  (https://github.com/samsonjs/strftime)  

```javascript
var net = require('net');  
var server = net.createServer(listener); 

function listener(socket) {  
  new Date();
  var currentTime = new Date();
  var year = 'YYYY';
  var month = 'MM';
  var date = 'DD';
  var hours = 'hh';
  var minutes = 'mm';
  
  year = currentTime.getFullYear().toString();  
  month = currentTime.getMonth();     // starts at 0 
  month = month +1;
  if (month.length < 2) {month = "0".concat(month.toString());}
  date = currentTime.getDate().toString();      // returns the day of month  
  hours = currentTime.getHours().toString();  
  minutes = currentTime.getMinutes().toString(); 
  var summeryTime = year + '-' + month + '-' + date +' ' + hours +":"+ minutes + "\n";
     
   //socket.write(summeryTime, 'utf8');
   socket.end(summeryTime);
 }

 server.listen(process.argv[2]);  
```
  



