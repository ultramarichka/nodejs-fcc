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

 ## 7.HTTP CLIENT

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
  



