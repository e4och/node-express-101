# Node.js & Express.js for Hackers 101 Notes

**Made with ❤️ by Enoch**

Welcome to **Node.js & Express.js for Hackers 101 Notes**, a comprehensive guide designed to provide a solid foundation in Node.js and Express.js from a security perspective. This project is geared towards beginners and security enthusiasts who want to dive deeper into backend technologies and understand the nuances of web application security.

## What You’ll Learn

- **Basics of Node.js**: Understand the core concepts of Node.js, its asynchronous nature, and how it differs from traditional server-side technologies.
- **Express.js Fundamentals**: Learn about Express.js, the de facto standard for Node.js web applications, and how to create robust and scalable web servers.
- **Practical Examples**: Explore real-world examples and code snippets that illustrate key concepts and best practices.

## Why This Project?

This project was created to help bridge the gap between theoretical knowledge and practical application. As a security enthusiast, I’ve compiled these notes to provide a beginner-friendly yet detailed resource for anyone interested in web and application security. My goal is to make these concepts accessible and easy to understand, ensuring that you can apply them effectively in your own projects.

# PREREQUISITES

Before delving into advanced JavaScript topics such as Node.js and Express.js, it is essential to establish a strong foundation in fundamental JavaScript concepts. This will not only enhance your comprehension of these advanced topics but also streamline the learning process.

- **HTML**
- **CSS**
- **Essential JavaScript concepts like Promises, Async/Await, Callbacks, Destructuring, Classes & Objects, Commonly used methods and a good understanding of ES6 Syntax**

# What the heck is Node.js anyway?

Node.js is a JavaScript runtime environment revolutionising server-side development with its event-driven, non-blocking architecture.

# Modules

In a way, Modules let you organise your code . You can split parts of you code and store them in a different file and access the code inside it from another files. For example if you have to store some variables with people’s names in it, then you can create a new file for it and separate them from the main code file. Doing this reduces complexity of the code and makes it neat.

Let’s look at an example and see how modules work

For example, Let’s say we have two files, one called names.js and another called app.js

<mark style="background: #CACFD9A6;">names.js</mark>

```jsx
const user1 = "Enoch";
const user2 = "Sid";
const user3 = "Diya";

module.exports = { user1, user3 }; //exporting it to make it available outside
```
****
<mark style="background: #CACFD9A6;">app.js</mark>

```jsx
const names = require("./names"); //requiring it to be able to access the code

console.log(names.user3);
console.log(names.user2); //trying to access code that was not exported

OUTPUT:

Diya
undefined
```
****
As we can see, to make the code visible to the other files we need to use the exports method and pass in the code we want to make it available, which gives us total control what to export and what not to. And wherever we need to use the code we just need to use the require function and pass in the path to the file we want to access code from. We are storing the **`require()`** function to a variable to make it easier to access the code inside the **`names.js`** file When we try to access code that was no exported, we get an **‘undefined’** as you can see in the example code

> 💡 **Modules work with functions and objects too**

<mark style="background: #CACFD9A6;">function.js</mark>

```jsx
function sayHello(){
    console.log("Hello")
}

module.exports = sayHello()
```
****
<mark style="background: #CACFD9A6;">app.js</mark>

```jsx
const notMyCode = require('./function');

notMyCode.sayHello
```

```scala
OUTPUT:

Hello
```
****
## Modules with objects

<mark style="background: #CACFD9A6;">hello.js</mark>

```jsx
const someRandomObject = {
    name:"Hello World!",
    saySomething: () => {
        console.log("GOODBYE WORLD!")
    }
}

module.exports = someRandomObject
```
****
<mark style="background: #CACFD9A6;">app.js</mark>

```jsx
const greet = require("./hello");

console.log(greet.name); 

greet.saySomething(); //using the function inside the object

```

```scala
OUTPUT:

Hello World!
GOODBYE WORLD!
```
****
# Node Built in Modules

There a multiple Built-in Modules in Node but we will just look at some of them.
## HTTP Module

The HTTP module lets us make HTTP requests. Because it’s an inbuilt module, we can simply require it and use it. The HTTP module has a various methods to handle requests and responses to our node application. Let’s look at an example:

```jsx
const http = require('http'); //requiring the http module

const server = http.createServer((req,res) => { //creating a server variable
    if (req.url === '/'){
        res.write("This is my first node.js app")
        res.end() //we need to end the response once we are done
    }else{
        res.write("Access Denied")
        res.end()
    }
})

server.listen(8080) //setting up a port to run the server on

/*Pay attention to the createServer() method we used, it takes in two 
parameters.REQ is for request and RES for response. 
We used an if statement to respond if a request with a message if the request 
is being made to the home directory ("/")
Incase the request is being made to another locations other than the home
directory, we simply respond with a message saying "Access Denied" */
```
****

**A BETTER WAY OF USING RES AND REQ**

```jsx
const http = require('http');

const server = http.createServer((req,res) =>{
    if (req.url === '/'){
        res.end("This is my first node.js app")
    }else{
        res.end("Access Denied")
    }
})

server.listen(8080)

/*Notice how we just used the end() method instead of using the write() method?
This is because we can just end the HTTP connection with a mesage instead
of using the write() method*/
```

>**💡 We can also pass in responses as HTML code**

****
## FS Module

The FS module short for **FILE SYSTEM**, let’s us read and write to file on the device. There are two ways of using the File System. The first one is the synchronous method and the other way is asynchronous method. Let’s look at how we can use both of them:

### FS Module Synchronous

This way of using the File system lets us read and write to file in a synchronised fashion (Blocking) Before we start using the FS Module, we have to import it as usual because it’s an inbuilt module

#### Reading from a file
 
```jsx
const {readFileSync} = require('fs') //using destructuring

const first = readFileSync('./first.txt','utf-8')
console.log(first)

const second = readFileSync('./second.txt','utf-8')
console.log(second)

```

```scala

FILE CONTENT:

This is the first text
This is the second text

```

>**Notice that we provided `utf-8`as a parameter to the `readFileSync()` function that is because if we do not provide the file encoding it would simply return a buffer and it wouldn't be useful for us, So for that reason we need to  provide file encoding type when we read from a file using the FS module**

****
#### WRITING TO A FILE
```jsx
const {writeFileSync} = require('fs') //using destructuring

const firstWrite = writeFileSync('./write.txt','Hello')
console.log(firstWrite)
```

```scala
FILE CONTENT:

Hello
```
****
#### APPENDING TO A FILE

```jsx
// We can also append to an existing file. To do this, we must use a flag

const {writeFileSync} = require('fs')

const firstWrite = writeFileSync('./write.txt'," I'm Enoch",{flag: 'a'})
//The file already contains the text "Hello" in it, now we are appending to it

console.log(firstWrite)

//Notice the flag we used. We used 'a' for the flag to append the text

```

```scala
FILE CONTENT:

Hello I'm Enoch 
```
****
### FS Module Asynchronous

This way of using the File system lets us read and write to file in a asynchronous fashion (Non-Blocking). Using the FS module asynchronously is a somewhat different from how we used the **FS** module using the synchronous method.

First of all the variable name is different if you notice, it’s just **`readFileSync()`** instead of **`readFileSync()`** which we used when working with the **FS** Module in the synchronised fashion. Secondly, we are passing in a call back function inside the **`readFileSync()`** function with the two parameters “**err**” and “**result**”. 
We use a callback function with these parameters to check if any errors occurred when we were trying to read the file **“ASYNCHRONOUSLY”**

> 💡 **We have to provide a callback function to the readFile() function because we are dealing with both reading and writing to a file. This is because we are dealing with ASYNCHRONOUS Code**

> ⚠️ **Always remember that we can access the call back variables only within the callback itself. If we try to access the variable data from outside the callback function block we will get an error because we cannot read the variables from outside the callback function block**

#### READING FROM A FILE
```jsx

const {readFile} = require('fs') //using destructuring

readFile('./first.txt','utf-8', (err,result) =>{   //passing a callback
//incase of error
    if(err){
        console.log("An error was occured. Details below:")
        console.log(err)
    }else{  //if no error
        console.log(result) //must be inside the callback function block
    }
});

//Notice the callback function we provided to the readFile() function
//Also notice that we provided the encoding format which is important
```

```scala
FILE CONTENT:

This is the first text
```
****
### WRITING TO A FILE
```jsx

const {writeFile} = require('fs') 

writeFile('./first.txt', 'Hello World!', (err,result) =>{
    if(err){
        console.log(`An error has occured: ${err}`)
    }else{
        console.log(`File written successfully`)
    }
});
```

```scala
FILE CONTENT:

This is the first text
```
****
#### We can clean this code up a little by using **`PROMISES`** and **`ASYNC/AWAIT`** Let's use **`PROMISES`:**

**WRITE TO A FILE ASYNCHRONOUSLY AND USING PROMISES**
```jsx
const {writeFile} = require('fs') 

const write = new Promise((resolve,reject) =>{
    writeFile('./first.txt',"Bye", (err,result) =>{
        if(err){
            reject(err)
        }else{
            resolve("File written successfully")
        }
    });
});

write.then((data) => {
    console.log(data)
}).catch((data) =>{
    console.log(data)
});
```

```scala
OUTPUT: 

File written successfully
```
```scala
FILE CONTENT:

BYE
```
****

**READING A FILE SYNCHRONOUSLY AND USING PROMISES**

```jsx
const {readFileSync} = require('fs')

let data = NaN
const read = new Promise((resolve,reject) =>{
    if(data = readFileSync('./first.txt','utf-8')){
        resolve(data)
    }else{
        reject
    }
});

read.then((data) =>{
    console.log(data)
}).catch(() => {
    console.log("There was an error reading the file")
});
```

```scala
OUTPUT: 

HELLO WORLD
```

```scala
FILE CONTENT:

HELLO WORLD
```
****
# Using the Node Package Manager (NPM)

The Node Package Manager (npm) is a vast repository of JavaScript packages that have been created by other developers. It enables us to reuse code written by other developers in our own projects, facilitating quicker development. NPM simplifies the process of finding, installing, and managing these packages, making it an essential tool for JavaScript developers. The URL for NPM is [**www.npmjs.com**](http://www.npmjs.com/)

> **💡 When we install NODE, NPM is automatically installed along with the**
## Installing Node packages

A node package can be installed for two purposed basically. We can either install a node package as a local dependency meaning that we are going to use it for one particular project or we can install the package globally and that allows us to use it in any project. Let’s look at how we can do both
### As local dependency
```node
npm -i <packageName>
```

### As global dependency
```node
npm install -g <packageName>
```

## package.json file

Along with installing the node packages, we also need to set up a **package.json** file which essentially is a manifest file that contains crucial information about our project we are building. There are two different ways to set up the **package.json** file. Let’s look at it:
## Manual Approach

In this approach we have to fill up the details for the package manually like setting up the author’s name, project description etc

```node
npm init
```

## Default Approach

In this approach the details for the **package.json** file are filled automatically and we can start working on the project right away without the need for manually filling up the details

```node
npm init -y
```

# Event Loops

In situations where there are asynchronous JavaScript code which usually takes some times to execute, we do not want the code below it to pause and wait for the asynchronous code to finish executing. This will cause the application to slow down. Event loops helps us solve this issue. Event loop will unload the code temporarily and will execute the below code first and after their execution and when there are no immediate code present, Then the asynchronous code will be executed. Event loops play a crucial role in maintaining responsiveness and preventing the application from freezing while asynchronous operations are in progress. Let’s understand this with an example:

```jsx
setTimeout(() => {
    console.log("I should be displayed first");
},0)
console.log("I should be displayed second")
```

```scala
OUTPUT:

I should be displayed second
I should displayed first
```
****
> **If you look closely, even though the setTimeout() function has a delay of 0 seconds, the code below it executed first. JavaScript unloads code which are asynchronous no matter how short the wait period.**

> **This is relevant to the SetInterval() function too**!

```jsx
setInterval(() => {
    console.log("Hello")
},2000)
console.log("Goodbye")
```

```scala
OUTPUT:

Goodbye
Hello
Hello
Hello
Hello
......keeps on going //because it's setInterval()
```
****
# Listening for Events in Node.js

Similar as we were able to listen for events in vanilla JavaScript using the **`addEventListener()`** method, we can also listen to events using Node.js. Let’s look at how we can do that:

```jsx
const eventEmitter = require('events') //requiring the events module
const event = new eventEmitter() //making an instance of the eventEmitter

event.on("startCar", () =>{
    console.log("Vroom Vroom!")
});

event.emit("startCar") //triggerring the event
```

```scala
OUTPUT:

Vroom Vroom 
```
****
> **Notice how we had to make a new instance of the eventEmitter. That's because the events module returns a class which has various functionalities to work with events and respond to the events And similar to the addEventListener() method in vanilla JavaScript, we can have multiple event listeners for the same event type**

>**Also, we can pass in multiple arguments inside the `emit()` or the request method and capture it as parameters from the listener or the `on()` method. Let’s look at an example:**

```jsx
const eventEmitter = require('events');
const event = new eventEmitter();

event.on("startCar", (carManufacturer,CarVariant) =>{
    console.log("Vroom Vroom!")
    console.log(`The car is built by ${carManufacturer} 
		and the variant is ${CarVariant}`);
});

event.emit("startCar","Lamborghini","Urus") //passing multiple arguments
```

```scala
OUTPUT:

Vroom Vroom!
The car is built by Lamborghini and the variant is Urus
```
****
# Streams Module

The Streams Module is used to read or write sequentially or in a continuous fashion. We use this when we have to work with streaming of data.

## Types of streams in Node.js

#### In Node.js we have 4 different types of steams:

> **Writable** - It is used to write data sequentially 
> **Readable** - It is used to read data sequentially 
> **Duplex** - It is used to both read and write data sequentially 
> **Transform** - It is used when we want to modify the data while the data is being read or written

#### For this example we are going to use the **`createReadStream()`** function which we will require from the **FS** module

**Let’s look at some code examples:**

```jsx
const {createReadStream} = require('fs'); //using destructuring

const stream = createReadStream('./first.txt'); //reading from a file

stream.on('data', (result) =>{     //listening to events
    console.log(result);
});
```

>**In the above code we are requiring the `createReadStream()` function from the FS module. This function is used to read data from a file sequentially. Below that we are using the `on()` method to listen for events and resulting which the data is displayed using the console log. We can also provide the file encoding format as a parameter to the `createReadStream()` function for the data to be displayed properly.**

****
##### A few important things to keep in mind when working with `createReadStream()` function:
- A buffer is a is used to temporarily store data that is being transferred or processed in a stream.
- When data is written to the stream, it is first stored in the buffer. When the buffer is full, the data is read from the buffer and displayed as outputs
- The default size of the buffer is **64 KB**
- It is possible to control the size of the buffer using the **`highWaterMark`** object as a parameter to the **`createReadStream()`** function.

##### Let’s see a code example which uses the **`highWaterMark`** object to set custom buffer size:

```jsx
const {createReadStream} = require('fs');

const stream = createReadStream('./first.txt', {highWaterMark: 8000});

stream.on('data', (result) =>{
    console.log(result);
})

```

```scala
OUTPUT:

<Buffer 48 65 6c 6c 6f 20 57 6f 72 6c 64 20 30 0a 48 65 6c 6c 6f 20 57 6f 72 
6c 64 20 31 0a 48 65 6c 6c 6f 20 57 6f 72 6c 64 20 32 0a 48 65 6c 6c 6f 20 57 
6f ... 7950 more bytes>
<Buffer 36 0a 48 65 6c 6c 6f 20 57 6f 72 6c 64 20 35 30 37 0a 48 65 6c 6c 6f 
20 57 6f 72 6c 64 20 35 30 38 0a 48 65 6c 6c 6f 20 57 6f 72 6c 64 20 35 30 39
0a ... 7857 more bytes>
```

>**The size of the file that was being read was `16KB`, so the output was split into two buffers and was displayed twice because we gave the custom buffer size of `8KB`**

>**We can also pass in the encoding format which we want to use when the file is being read by the `createReadStream()` function. We can pass it in along with the other parameters inside the object that’s inside the `createReadStream()` function**

```jsx
const {createReadStream} = require('fs')

const stream = createReadStream('./first.txt', {
highWaterMark: 8000,
encoding: 'utf8'
});

stream.on('data', (result) =>{
    console.log(result)
});
```

****

>**Since we are using the `eventEmitter()` to listen for events, we can also catch errors that may occur. We can make use of the `on()` method on the variable used to read the file, to do this task**

```jsx
const {createReadStream} = require('fs');

const read = createReadStream('../first.txt',{   //intentionally creating an error
    highWaterMark:8000,
    encoding: 'utf8'});

read.on('data', (info) => {
    console.log(info)
});

read.on('error', (err) => { //checking for errors
    console.log("An error has occured, Details below:")
    console.log(err)
});
```

```scala
OUTPUT:

An error has occured, Details below:
[Error: ENOENT: no such file or directory, open '../first.txt'] {
  errno: -2,
  code: 'ENOENT',
  syscall: 'open',
  path: '../first.txt'
}
```

> **In the above code, we get an error because we have wrongly specified the location of the file we need to read from**

****

Let’s look at some code that uses streams to send data in chunks. We will use both ways of sending data from the server all at once and after that how we can use the streams module to send the data in chunks which is the recommended way of sending huge amount of data from the server
##### TRADITIONAL WAY OF SENDING DATA FROM THE SERVER ALL AT ONCE

```jsx
const http = require('http');
const fs = require('fs');

http.createServer((req,res) =>{
    const data = fs.readFileSync('./first.txt', 'utf-8')
    res.end(data) //ending the connection right away after sending the data
    })
    data.on('error', (err) =>{
        console.log("Oops, some errors has occured!")
        res.end(err)
    })
}).listen(8080,()=>{
    console.log("Server Started on port 8080...")
});
```
##### USING THE STREAMS MODULE TO SEND DATA IN CHUNKS
```jsx
const http = require('http');
const fs = require('fs');

http.createServer((req,res) =>{
    const data = fs.createReadStream('./first.txt','utf-8')
    data.on('open', () =>{
        data.pipe(res) //helps to send the data in chunks
    });
    data.on('error', (err) =>{
        res.end(err)
    });
}).listen(8080,()=>{
    console.log("Server Started on port 8080...")
});
```

>**In the above code using the streams module, we are sending the data in chunks, we have used the `pipe()` method so that the data can be passed in as chunks and not all at once. we also have passed in the res inside the `pipe()` method as a parameter indicating that we want to end it, after all the data is sent**

****
# HTTP HEADERS

All along we have been responding only with the contents to requests made to our server. We never really looked at how we can set custom headers which will provide more information, like the file type, the status codes etc.. These values are set by the server by default.
##### Let’s see how we can set custom HTTP Headers.

```jsx
const http = require('http');

const server = http.createServer((req,res) =>{
    res.writeHead(200,{"content-type":'text/html '})
    res.write('<h1>Hello There!</h1>');
    res.end();
});

const serverPort = 8080
server.listen(serverPort,() =>{
    console.log(`Server started on port ${serverPort}`);
});
```

>**Pay attention to the line of code highlighted in red. We have used a method called `writeHead()` on the response. This method is used to set custom values to the header of the response that the server provides to the request. This method can contain HTTP Status codes, file-type and much more.**

>**💡 Normally Express.js would take care of the headers for us. But we can use this to set custom headers if we want it that way**

****
#### Let’s put it all together and make a simple web server to handle requests

```jsx
const http = require('http');
const fs = require('fs');

// A function to handle requests when there is a server side error
function resourceUnavailable(res){
    res.writeHead(503,{"content-type":'text/html '}) //sending status codes
    res.write("<h1>There was an error, Contact the Server Admin<h1>")
    res.end();
}

// A function to handle successful requests to the server
function resourceAvailable(res,success){
    res.writeHead(200,{"content-type":'text/html '}) //sending status codes
    res.write(success)
    res.end();
}

const server = http.createServer((req,res) =>{
    if(req.url === '/'){
        fs.readFile('./html/home.html', (err,success) =>{
            if(err){
                resourceUnavailable(res)
            }else{
                resourceAvailable(res,success)
            }
        });
    }else if(req.url === '/about'){
        fs.readFile('./html/about.html', (err,success) =>{
            if(err){
                resourceUnavailable(res)
            }else{
                resourceAvailable(res,success)
            }
        });
    }else if(req.url === '/contact'){
        fs.readFile('./html/contact.html', (err,success) =>{
            if(err){
                resourceUnavailable(res)
            }else{
                resourceAvailable(res,success)
            }
        });
    }else{
        res.writeHead(404,{"content-type":'text/html '})
        res.write("<h1>RESOURCE NOT FOUND!</h1>")
        res.end()
    }
});

const serverPort = 8080
server.listen(serverPort,() =>{
    console.log(`Server started on port ${serverPort}`)
});
```

# Express.js

Express.js is a minimal and flexible framework for Node.js. It works on top of the Node.js web server. It’s main purpose is to simplify creating web servers and it also provided multiple essential functionalities that further make the creating of a web server easy and robust. The syntax for Express.js is similar to how we write Node.js but with minor differences.

> ⚠️ **Before we can use express, we need to install it. The command to do that is `npm install express`**

****
## Reasons to use express

- **Routing**: Defining routes for different URLs and associating them with specific handler functions.
- **Middleware:** Interceptors that can process requests and responses before they reach your application code.
- **Templating**: Tools for generating HTML dynamically based on data.
- **Error handling:** Mechanisms for catching and handling errors gracefully.
- **Session management**: Functionality for storing user data between requests.
- **Performance:** Express.js is known for its high performance and scalability.
- **Community:** It has a large and active community, which means you can easily find help and support.
- **Documentation:** The official documentation is comprehensive and well-written.

> 💡 **Earlier with Node.js we had to use the HTTP module to create a server, but with Express.js we don’t need it. Express has built in functionalities to handle the HTTP protocol**

****
### Let’s create a simple HTTP server using express

```jsx
const express = require('express');
const app = express();

app.all('*', (req, res) => {
    res.status(404).send("<h1>Resource not found!</h1>");
});

app.get('/', (req, res) => {
    res.status(200).send("Home page");
});

app.get('/about', (req, res) => {
    res.status(200).send("About page");
});

const port = 8000;
app.listen(port, () => {
    console.log(`Server started on port ${port}`);
});
```
#### A Few important things to note here:

>First of all, if you notice, we have not used the HTTP module to create a server and handle requests and responses, express takes care of this. Secondly notice the method **app.all()**

>We can send a response whenever the users try to access resources that are not available by using this method. This syntax is more readable, easy to understand and implement compared to writing the if and else logic.

>We are also using the **get()** method to handle **GET** requests to the server. And also we are using the **send()** method instead of using the **end()** and **write()** method.

>If you look closely, you can see that we have chained the **status()** method with the **send()** method, Thus making the code look more clean and readable

****
#### A simple HTTP server that responds with HTML files


```jsx
const express = require('express');
const path = require('path') //using path module to work with file paths
const app = express();

app.get('/',(req,res) =>{
    res.sendFile(path.resolve('./html/home.html'));
    res.status(200);
});

app.get("/about",(req,res) =>{
    res.sendFile(path.resolve('./html/about.html'));
    res.status(200);
});

app.all('*',(req,res) =>{
    res.send("<h1>Resource not found!</h1>").status(404)
});

const port = 8080
app.listen(port,()=>{
    console.log(`Server started on port ${port}`)
});
```

> **We have required the path module so that we can work with file paths which will allow us to send HTML files are responses. Take a closer look at the `path.resolve()` method. This method basically converts the relative file path to an absolute file path which will help the server in sending the HTML file**

****
# Static Files

It is common to have resources like CSS, HTML, images, text files in our web application. Since these are static and do no change, we can add them to a folder and serve them to the server in one shot, instead of using the **`get()`** method multiple times to send files to the server for it to load the resource and run our web application properly. Since these are just static files it would be a good idea to put them all into a folder which would make the code base look cleaner and neat.

> 💡 **We have to use the `app.use()` method to be able to serve the static files to the server.**
## Advantages of storing static resources in a dedicated folder

1. **Code Organisation:** Storing static resources in a dedicated folder promotes code organisation and maintainability, making it easier to locate and manage these files.
2. **Performance Optimisation:** By serving static files directly from a dedicated folder, you can bypass the need for the server to process requests for these resources, leading to improved performance.
3. **Caching:** Static files can be cached by browsers and web servers, further enhancing performance and reducing server load.
4. **Simplification:** By managing static files separately, you can simplify the routing and handling of dynamic content, such as server-side generated pages or API responses.
5. **Scalability:** As your web application grows, managing static files in a separate folder helps maintain scalability and efficiency.

> ⚠️ **It’s crucial to have the `app.use()` method above all the other route-specific methods, such as `app.get()`, `app.post()` etc. If you place the `app.use()` method after other route-specific methods, it will never be reached.**
## Enough theory, let’s see a practical example

### The Bad Approach

<mark style="background: #CACFD9A6;">index.html</mark>

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HOME</title>
    <link rel="stylesheet" href="home.css">
</head>
<body>
    <h1>Welcome to the home page</h1>
    <img src="cat.png" alt="cat image">
</body>
</html>
```

**BAD APPROACH OF SENDING RESOURCES INDIVIDUALLY**

```jsx
const express = require('express');
const path = require('path');
const app = express();

app.get('/home.css',(req,res) =>{
    res.status(200).sendFile(path.resolve('./static/home.css'));
}); //sending the css file individually

app.get('/cat.png',(req,res) =>{
    res.status(200).sendFile(path.resolve('./static/cat.png'));
}); //sending the png file individually

app.get('/',(req,res) =>{
    res.status(200).sendFile(path.resolve('home.html'));
});
app.all('*',(req,res) =>{
    res.status(404).send("<h1>Resource not found!</h1>");
});

const port = 8080
app.listen(port,()=>{
    console.log(`Server started on port ${port}`);
});
```

>**If you pay attention to the code, we are sending the static files individually. This isn't an issue when we don't have multiple static files but in case of bigger projects, it would just not be efficient to write code to respond to each individual resource request**

****

**EFFICIENT APPROACH OF SENDING RESOURCES USING STATIC**

```jsx
const express = require('express');
const path = require('path');
const app = express();

app.use(express.static('./static'))  //all static files in a folder called "static"

app.get('/',(req,res) =>{
    res.status(200).sendFile(path.resolve('./home.html'))
});

app.all('*',(req,res) =>{
    res.status(404).send("<h1>Resource not found!</h1>")
});

const port = 8080
app.listen(port,()=>{
    console.log(`Server started on port ${port}`)
});
```

>Have a look at the highlighted code. By using the **`express.static()`** method, we can make all the static resources available to the server instead of setting up each resource to be available one by one.

>To be honest, for me personally the way the **`express.static()`** method works feel like magic. It takes care of the file paths, it takes care of the status codes and all those boring stuff. But it's important to note that these are made possible by the underlying code written by developers who developed express, thus making our work considerably easier.

> ⚠️ Please note that when you have a home page and you want it to show up directly without the **URL** having the file name of the home page, it’s important to name the home page as **`index.html`**, so when express sees this file, it will the page automatically when the **URL** is visited and it wouldn’t contain **index.html** in the **URL.**
# JSON Basics

JSON stands for **JavaScript object notation**. It’s basically a text format that is completely language independent. It is commonly used for transmitting data in web applications. For example: Sending some data from the server to the client, so it can be displayed on a web page, or vice versa. We can use express to respond to HTTP requests with JSON data. Let’s see an example:

```jsx
const express = require('express');
const app = express();

app.get("/",(req,res) =>{
  res.json([{name:'Enoch',
            age: 21,
            sex: 'Male',
            Working: true},
            
            {name: 'Annie',
             age: 25,
             sex:'Female',
             Working: false }
          ]);
});

const port = 8080
app.listen(port,() =>{
  console.log(`Server started on port ${port}...`)
});
```

> As you can see, we have two JSON objects with some values which are  enveloped by an array. We use the **`res.json()`** method to send the JSON data

```scala
OUTPUT:

[{"name":"Enoch","age":21,"sex":"Male","Working":true},
{"name":"Annie","age":25,"sex":"Female","Working":false}]
```

****
## Responding with only minimal data from JSON objects

Although we may have a lot of data values in our JSON data, we can choose to respond with only a few of them. We do not need to send the entire values inside the JSON objects. This will enable accessing of data which are only required and helps keep the response clutter-free and minimal. Let’s see how we can do this with an example. We have 2 files, one named as **`data.js`** which will hold the JSON objects with their values and our **`index.js`** which will be our main file where we write out JavaScript to perform handle requests and responses.

<mark style="background: #CACFD9A6;">data.js</mark>

```javascript
const enoch = {
  name:'Enoch',
  age: 21,
  sex: 'Male',
  working: true
}

const annie = {
  name: 'Annie',
  age: 25,
  sex:'Female',
  working: false 
}

const users = [enoch, annie]; //storing the objects to an array

module.exports = {users};     // exporting the "user" object
```

****

<mark style="background: #CACFD9A6;">app.js</mark>

```jsx
const express = require('express');
const app = express();
const {users} = require('./data'); //requiring the object that was exported

app.get('/test',(req,res) =>{
    const data = users.map((specific) =>{ 
    const {name,age,sex, working} = specific; //storing values from JSON object
    return {name,sex,working} //only returning some of the values
  });
  res.json(data); //responding to the request with the "data" variable
});

const port = 8080;
app.listen(port,() =>{
  console.log(`Server started on port ${port}`);
});
```

```scala
OUTPUT:

[{"name":"Enoch","sex":"Male","working":true},
{"name":"Annie","sex":"Female","working":false}]
```

****

# Route Parameters

Let’s say we have an express server with a database, And the purpose of our server is to return the requests with the queried data, so if one of the users were to requests some data about X, we return the request with X’s data. Normally if we were to have only a few collection of data, we could just setup the **`get()`** method with the specific URL and respond to the request using the **`find()`** method. Let’s look at an example:

```jsx
const express = require('express');
const app = express();
const {users} = require('./data');

function search(users){
  return users.name === "Enoch" /*return entire enoch object if users.name 
																  is Enoch*/
}
app.get('/enoch_age',(req,res) =>{
  const enoch = users.find(search);
  res.json(enoch.age);
}) 

const port = 8080;
app.listen(port,() =>{
  console.log(`Server started on port ${port}...`)
});

```

<mark style="background: #CACFD9A6;">data.js</mark>

```javascript
const enoch = {
  name:'Enoch',
  age: 21,
  sex: 'Male',
  working: true
}

const annie = {
  name: 'Annie',
  age: 25,
  sex:'Female',
  working: false 
}

const users = [enoch, annie]; 
module.exports = {users};
```

>But this approach would not be feasible if we were to have a large amount of data and if we wanted to respond to all the requests with the data, it would be tedious and inefficient to write **`get()`** method for each possible requests and use the **`find()`** method to search and send the data. To solve this issue, we use **ROUTE PARAMETERS.**

Route Parameters basically takes the request from the **`get()`** function and pass it on to the search function and without the need to specify a single name for the find option. It’s dynamic so it saves us from specifically writing out static responses. Let’s look at an example

```jsx
const express = require('express');
const app = express();
const { users } = require('./data');

// Route for handling GET requests to /:name
app.get('/:name', (req, res) => {
  // Extract the name parameter from the URL
  const { name } = req.params;

  // Find the user whose name matches the parameter
  const data = users.find((a) => a.name === name);

  // If a user is found, send their data as JSON response
  if (data) {
    res.json(data);
  } else {
    // Handle the case where no user is found
    res.status(404).send('User not found');
  }
});

const port = 8080;

app.listen(port, () => {
  console.log(`Server started on port ${port}...`);
});

```

> **Notice how we implement route parameters. We add a Colon to indicate the endpoint followed by the name for it**

<mark style="background: #CACFD9A6;">data.js</mark>

```javascript
const users = [
  {
    name: 'enoch',
    age: 22,
    sex: 'Male',
    working: false
  },
  {
    name: 'annie',
    age: 34,
    sex: 'Female',
    working: true
  },
  {
    name: 'ram',
    age: 14,
    sex: 'Male',
    working: false
  },
];

module.exports = {users}
```

****
## Here is a breakdown of the code

### Importing and Destructuring Data:

- We're importing an object from a file named `data` using `require`.
- We're then destructuring the imported object and assigning the value of the key "users" to a variable named `users`.
### **Route Parameter and Request Handling:**

- We're using the `app.get` method to define a route that handles GET requests to the URL path `/:name`.
- This route utilises a route parameter named `name`.
- Inside the route handler, we're destructuring the value of the `name` parameter from the `req.params` object.
### **Finding Data and Sending Response:**

- We're utilising the `find` method on the `users` array to locate a user whose name matches the `name` parameter value.
- We're employing a callback function that receives each user object as the parameter  " `a` "
- Within the callback, we verify if the user's name aligns with the `name` parameter value.
- If a user is located, the `find` method returns that user object, which is stored in the `data` variable.
- Finally, we utilise **`res.json()`** method to transmit the `data` variable as a JSON response to the client.
# Query Parameters

Query parameters are key-value pairs appended to the URL after a question mark (?). They provide a way to send additional information to the server along with the URL path. The server can then access these parameters and use them to modify its behaviour.

> ⚠️ **It's crucial to define a specific route for the query parameter you want to handle. If you don't, Express.js will route the request to the first matching route regardless of the presence or value of the query parameter**

Let’s understand it well with an example:

```jsx
const express = require('express'); 
const app = express();
const { users } = require('./data');

//Define a route for any path with a parameter (/:route) and handle GET requests
app.get("/:route", (req, res) => {
  // Extract the "query" parameter from the URL query string
  const { query } = req.query;

  // Convert the "query" string to an integer using parseInt
  const formattedData = parseInt(query);

  // Check if the conversion was successful (not NaN)
  if (!isNaN(formattedData)) {
    // Filter the "users" array for users with age >= formattedData
    const resultData = users.filter((user) => user.age >= formattedData);

    // Send the filtered users as the response with a 200 (OK) status code
    res.status(200).send(resultData);
  } else {
    /* If conversion failed or query parameter was invalid, send a 404 
	(Not Found) response */
    res.status(404).send("No results found");
  }
});

const port = 8080; 
app.listen(port, () => {
  console.log(`Server started on port ${port}...`);
});
```

##### HTTP REQUEST: <http://localhost:8081/route?query=20

```scala
OUTPUT:

[{"name":"enoch","age":22,"sex":"Male","working":false},
{"name":"annie","age":34,"sex":"Female","working":true},
{"name":"sonny","age":44,"sex":"Male","working":true}] 
```

>**Since we give the query value as 20, all the objects with people who's age is >= 20 will be displayed**

<mark style="background: #CACFD9A6;">data.js</mark>

```javascript
const users = [
  {
    name: 'enoch',
    age: 22,
    sex: 'Male',
    working: false
  },
  {
    name: 'annie',
    age: 34,
    sex: 'Female',
    working: true
  },
  {
    name: 'ram',
    age: 14,
    sex: 'Male',
    working: false
  },
  {
    name: 'sonny',
    age: 44,
    sex: 'Male',
    working: true
  },
  {
    name: 'Siya',
    age: 11,
    sex: 'Female',
    working: false
  }
];

module.exports = {users}
```

> **💡 We can have as many Query Parameters as we want. Just make sure that they are separated with the AND  symbol (&) Example: [`http://localhost:8081/route?query=20](<http://localhost:8081/route?query=20>)&anotherDamnQuery=randomStuff`**

****
# Middleware

Imagine that we have a web server and we have two routes, First one is the `/home` route and the other one is the `/about` route. We have written a lot of logic for the route `/home`. But we want the same logic to be applied to the **`/about`** route too. 

Initially we might think that we can just copy and paste the logic of the **`/home`** route to the **`/about`** route too. 

But let’s that we have 200 routes and we want to apply the same logic to all of them, Would you think that copy and pasting the logic for all those routes would be a good idea? We’ll definitely not, First of all the task would be repetitive and also the code would be clunky. 

How do we deal with this now? Well, we could just write a function that would have all the logic and call the function each time we need to use it. Brilliant right? Yeah i thought the same. Let’s see how we can do that with a code example:

**CODE WITHOUT THE USE OF MIDDLEWARE**

```jsx  
const express = require('express');
const app = express();

app.get('/home',(req,res) =>{
  const method = req.method //storing the request method
  const url = req.url //storing the request URL
  const year = new Date().getFullYear() //storing the current year

  console.log(method,url,year);
  res.send("Connection to the server ended successfully");
});

app.get('/about',(req,res) =>{
  const method = req.method
  const url = req.url
  const year = new Date().getFullYear();

  console.log(method,url,year);
  res.send("Connection to the server ended successfully");
});

app.get('/contact',(req,res) =>{
  const method = req.method
  const url = req.url
  const year = new Date().getFullYear()

  console.log(method,url,year)
  res.send("Connection to the server ended successfully")
});

const port = 8080;

app.listen(port, () => {
  console.log(`Server started on port ${port}...`);
});
```

```scala
OUTPUT:

GET /home 2023
```

> **Get is the request method, `/home` is the URL and 2023 is the year**

>**If you pay attention to the highlighted code, you can see that we have repeated the same logic for the below 2 routes (about and contact) as well. We don't want that, it makes the code look clunky and it's not efficient**

**CODE USING MIDDLEWARE**

```jsx
const express = require('express');
const app = express();

const logic = (req,res,next) =>{
  const method = req.method   //storing the request method
  const url = req.url         //storing the request URL
  const year = new Date().getFullYear() //storing the current year
  console.log(method,url,year)
  next()
}

app.get('/home',logic,(req,res) =>{
  res.end("Home page")
});

app.get('/about',logic,(req,res) =>{
  res.end("About page")
});

app.get('/contact',logic,(req,res) =>{
  res.end("Contact page")
});

const port = 8080;
app.listen(port, () => {
  console.log(`Server started on port ${port}...`);
});
```

```scala
OUTPUT OF <http://localhost:8080/home>
CONSOLE: GET /home 2023
BROWSER PAGE: Home page

CONSOLE OUTPUT OF <http://localhost:8080/about>
CONSOLE: GET /about 2023
BROWSER PAGE: About page

CONSOLE OUTPUT OF <http://localhost:8080/contact>
CONSOLE: GET /contact 2023
BROWSER PAGE: Contact page
```

> In the above code example, we have defined a function called logic and written  the logic we want to repeatinside it. For any route requiring this logic,  we can simply call the function between the req and res arguments. 

 >We also pass the next parameter because after executing the logic,  we need to end the request with a response. If we choose not to explicitly end the request within the function, and instead rely on the **res.end()** method inside the route, we need a way to ensure its execution after the function finishes. This is where the next function comes in. It takes care of executing the code written inside the route once the function
   has completed its task. 
  
>To make this work, we also need to call the **`next()`** function at the end of the logic function's code. 
  This signals to Express that we are finished with the logic and it should now execute the code inside the route that called the function. Pay attention to how cleaner the code looks from the code which was written without the use of middleware

**Okay, our code looks pretty neat and tidy. But let’s clean it up some more. We can do two things to make our code look more neat and concise. Let’s look at then**

## Moving the logic function to a separate file

It’s always a good idea to separate the core functions from the main code. We can use the **`module.exports()`** method to make the code available and we can just import it whenever we want by using the **`require()`** function.

**CODE AFTER MOVING THE LOGIC FUNCTION TO A SEPARATE FILE**

```jsx
const express = require('express');
const app = express();
const logic = require('./logic') //importing the file where our logic is

app.get('/home',logic,(req,res) =>{
  res.end("Home page")
})

app.get('/about',logic,(req,res) =>{
  res.end("About page")
})

app.get('/contact',logic,(req,res) =>{
  res.end("Contact page")
})

 
const port = 8080;

app.listen(port, () => {
  console.log(`Server started on port ${port}...`);
});
```

<mark style="background: #CACFD9A6;">logic.js</mark>

```jsx
const logic = (req,res,next) =>{
  const method = req.method 
  const url = req.url 
  const year = new Date().getFullYear() 
  console.log(method,url,year)
  next()
}

module.exports = logic; //exporting the file
```

## USING `app.use()` METHOD

Calling a function as a middleware wherever necessary is a good approach. But what if we were to have multiple middlewares? The code will not look clean if we were to call multiple middleware functions, So to solve that we can use the **`app.use()`** method and pass in the function as an parameter(**`app.use(logic)`**) So that by default the logic will be relevant to all the routes

> 💡 Remember, the placement position of the **`app.use()`** method is very important. If we place the **`app.use()`** method above other **`app.get()`** methods, then all the requests will be routed via the **`app.use()`** method.

We can enhance the working of **`app.use()`** method by specifying the routes where we want the method to work on. Example, if we write **`app.use('/api',logic)`** then the logic function will be passed on to all the requests on routes that comes after the **`/api`** route. You can now choose exactly where you want your middleware to work. This makes your code cleaner and easier to understand. Let’s look at an example:

```jsx
const express = require('express');
const app = express();
const logic = require('./logic');

app.use('/home/',logic);

app.get('/home',(req,res) =>{
  res.end("Home page");
});

app.get('/home/enoch',(req,res) =>{
  res.end("Enoch's Desktop Files");
});

app.get('/home/enoch/superSecretFolder',(req,res) =>{
  res.end("R2V0IGEgbGlmZSE=");
});
 
const port = 8080;

app.listen(port, () => {
  console.log(`Server started on port ${port}...`);
});
```


> If you look closely at the highlighted code, you will notice that we have only called the logic function on the **`app.use()`** method, which is placed above all the other **`app.get()`** methods. So by default all the routes which comes after the **`/home`** route will call the middleware **`logic()`** function whenever the routes after the **/home** route is visited. This is a much cleaner way to use middlewares as it removes the requirement for calling the **`logic()`** function on every route

# Intro to other common HTTP requests using POSTMAN

## POST REQUEST

Since we can't perform **`POST`** requests through the browser, we'll be using a tool called **Postman**
Postman is an API platform for developers. Although we could use a command-line tool called **`CURL`**, I'll keep this beginner-friendly. Postman would do the job just fine. Once you get it installed, then we can get started.

**`POST`** Requests are one of the popular methods supported by the **`HTTP`** protocol. We learned about the **`GET`** method which essentially requests data from a server, but the **`POST`** method also allows us to create or post data on a server. We can use the **`POST`** method in express.js the same way we use the get method. Let's look at a code example:

```jsx
const express = require('express');
const app = express();

app.use(express.urlencoded({extended: false})); 

const users = [
  { name: 'Enoch', age: '22' },
  { name: 'Ram', age: '14' },
  { name: 'Jeff', age: '56' }
]

app.post('/home',(req,res)=>{
  const data = req.body;    //storing the data being sent through POST method
  users.push(data);
	console.log(users)
  res.send("User created!");
});
 
const port = 8080;

app.listen(port, () => {
  console.log(`Server started on port ${port}...`);
});
```

> In the above code, we have an array called **`users`** and it has some values in it. Below that we have used the **`app.post()`** method and we have specified the route as **`/home`** As **w**e would be sending data using the **`POST`** method, we have defined a variable called data that will store the data and push it to the "**users**" array, We will be displaying the "**users**" array and once we do that we send a response saying "**User Created!**". We have used the **`req.body()`** method which will store the data that we are sending through the **`POST`** request. 

> Also notice the highlighted code, whenever we are sending post requests through a URL, we must always encode it so that the data is sent properly, for this we have to use the **`.urlencoded()`** method. Or else it would just return an empty object. It is middleware that parses data sent in the request body as URL-encoded form data

**Now since we got that out of the way, let’s look at how we can send data through the `POST` request using POSTMAN**.**

![image](https://github.com/user-attachments/assets/6bdf669b-d086-423d-858c-04640fc62675)


As you can see in the screenshot, we have sent a **`POST`** request to our **`/home`** route and we have also selected to send the data as **URL-encoded** format. Below that we have set 2 key and values, the name and the age. If you did everything right, and hit send, it should send the **`POST`** request successfully and the message called “User Created!” should be displayed. Let’s check our terminal to see if see the “**users**” array being displayed. The data we passed in through the **`POST`** request must be visible there.

**Voila!**
![image](https://github.com/user-attachments/assets/838c70f6-f246-4987-a715-85e5b700614f)


****
## PUT REQUEST

**The PUT request lets us update or create a resource on the server. Let’s look at an example:**

```jsx
const express = require('express');
const app = express();

app.use(express.urlencoded({extended: false}))

let database = [ //an array containing some data
  
    {userId: 1, userName: 'enoch', userAge: '22' },
    {userId: 2, userName: 'anny',  userAge: '28' },
    {userId: 3, userName: 'deepak',userAge: '14' },
    {userId: 4, userName: 'jerry', userAge: '77' },
    {userId: 5, userName: 'hari',  userAge: '34' }

]

app.put('/api/people/:id',(req,res) =>{
  const {id} = req.params  //destructuring and storing the id value from the url
  const {name} = req.body

  const isUser = database.find((person) => person.userId === Number(id))
//checks if there is a data in the database with the ID provided as input in url
  if(!isUser){
    res.status(404).send(`No data found on user with id: "${id}"`)
		//if not, then return this
  }else{
    isUser.userName = name
//update the name in the database with the provided name through the POST method
    res.status(200).json(database)
  }
})

const port = 8081;

app.listen(port, () => {
  console.log(`Server started on port ${port}...`);
});
```

#### Now let’s use postman to send a PUT request to update the name in the database with the `userId` of 4

![image](https://github.com/user-attachments/assets/51e0b39c-a979-42b8-b0da-c5a4f4bd57df)


> **As you can see in the screenshot, we have successfully update the data that was in the database array. The previous `userName` of `userId`: 4 was `jerry`”**

****
## DELETE REQUEST

The name of the request is self explanatory, It’s basically used to delete a resource from the server. Let’s look at an example:

```jsx
const express = require('express');
const app = express();

app.use(express.urlencoded({extended: false}));

const database = [
  
    {userId: 0, userName: 'enoch', userAge: '22' },
    {userId: 1, userName: 'anny',  userAge: '28' },
    {userId: 2, userName: 'deepak',userAge: '14' },
    {userId: 3, userName: 'jerry', userAge: '77' },
    {userId: 4, userName: 'hari',  userAge: '34' }
	
     //We have started the array index from 0 this time
]

app.delete('/api/people/:id',(req,res) =>{
  const {id} = req.params //destructuring and storing the ID value from the URL
  

  const isUser = database.find((person) => person.userId === Number(id))
	//checking if the user exists with the provided userID in URL of the request
  if(!isUser){
    res.status(404).send(`No data found on user with id: "${id}"`)
  }else{
    database.splice(id,1)
	
    res.status(200).json(database)
  }
});

const port = 8081;

app.listen(port, () => {
  console.log(`Server started on port ${port}...`);
});
```

> **Pay attention to the code, We are using the `splice()` method to remove a value from the array. ID is the index position and 1 is to say that we want to remove 1 value from the array, or it would delete the all the values from the array**

#### Let’s look the results from **POSTMAN**

![image](https://github.com/user-attachments/assets/3e346c92-9aed-4a1b-8de6-d17c9963a434)


> **The array value in the 0th position was `{userId: 0, userName: 'enoch', userAge: '22' }` but since we gave `0` in the request parameter, this value from removed from the array**

****
# Routes

Routes helps us organise our code, so that our main JavaScript file is not cluttered and messy. It would look clean. Suppose we have multiple routes in our web application and we want to be able to perform various functionalities with each route. As our routes grow, it will be difficult to maintain them if they are all in one single file, it would be a nightmare to debug and would be pretty confusing overall. So to solve this issue we can use routes to help us separate these routes to an another folder where we can have an individual file for each route which would be much easier to maintain. Let’s take a look at an example

> Let’s say we have 2 routes for a fruit store, we got apples and mangoes. We have to perform various functionalities on each route. So we can create a folder and name it something, let’s name it “**`route`**” for now, inside that we can create two files, on for the apple route and the other for mango route. Let’s look at the code inside each file

**`apple.js`**

```jsx
const express = require('express');
const router = express.Router();

router.get("/",(req,res)=>{
    res.send("Here is your apple");
});

router.post("/",(req,res)=>{
    res.send("Your apple has been sent");
});

router.put("/",(req,res)=>{
    res.send("Here is your new apple");
});

router.delete("/",(req,res)=>{
    res.send("Your apple was thrown away");
});

module.exports = router
```

**`mango.js`**

```jsx
const express = require('express');
const router = express.Router();

router.get("/",(req,res)=>{
    res.send("Here is your mango");
});

router.post("/",(req,res)=>{
    res.send("Your mango has been sent");
});

router.put("/",(req,res)=>{
    res.send("Here is your new mango");
});

router.delete("/",(req,res)=>{
    res.send("Your mango was thrown away");
});

module.exports = router
```

**`index.js`**

```jsx
const express = require('express');
const app = express();
const appleRoute = require('./routes/apple'); //importing the apple file
const mangoRoute = require('./routes/mango'); //importing the mango file

app.use('/apple',appleRoute); 
app.use('/mango',mangoRoute);

const port = 8080;

app.listen(port, () => {
  console.log(`Server started on port ${port}...`);
});
```

>Notice the highlighted code, we use the **app.use()** method and pass in the route names and right next to it we provide the value which holds in the  route location of those route names. Notice in the **`mango.js`** and **`apple.js`** file, we have not provided the route paths, because we are already providing the paths while we are requiring or importing the file. Also note that we are not using the usual **`app.get()`** method, instead we are using the **`router.get()`** method as we are using the route concept

```jsx
OUTPUT FROM VISITING:
<http://localhost:8080/mango>

Here is your mango
```

```jsx
OUTPUT FROM VISITING:*
<http://localhost:8080/apple>

Here is your apple
```

****
