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
