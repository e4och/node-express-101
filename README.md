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

In a way, modules let you organize your code. You can split parts of your code and store them in different files, then access the code inside them from other files. For example, if you need to store some variables with people's names in them, you can create a new file for that and separate them from the main code file. Doing this reduces the complexity of the code and makes it neat.
Let's look at an example and see how modules work.

For example, let's say we have two files, one called `names.js` and another called `app.js`.

<mark style="background: #CACFD9A6;">names.js</mark>

```jsx
const user1 = "Enoch";
const user2 = "Sid";
const user3 = "Diya";

module.exports = { user1, user3 }; // exporting it to make it available outside
```
****
<mark style="background: #CACFD9A6;">app.js</mark>

```jsx
const names = require("./names"); // requiring it to be able to access the code

console.log(names.user3);
console.log(names.user2); // trying to access code that was not exported
```

```scala
OUTPUT:

Diya
undefined
```
****
As we can see, to make the code visible to other files, we need to use the `exports` method and pass in the code we want to make available, which gives us total control over what to export and what not to. Wherever we need to use the code, we just need to use the `require` function and pass in the path to the file we want to access code from. We are storing the **`require()`** function to a variable to make it easier to access the code inside the **`names.js`** file. When we try to access code that was not exported, we get an **‘undefined’** as you can see in the example code.

> 💡 **Modules work with functions and objects too**

<mark style="background: #CACFD9A6;">function.js</mark>

```jsx
function sayHello() {
    console.log("Hello")
}

module.exports = sayHello()
```
****
<mark style="background: #CACFD9A6;">app.js</mark>

```jsx
const notMyCode = require('./function');

notMyCode.sayHello();
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
    name: "Hello World!",
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

greet.saySomething(); // using the function inside the object
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

The HTTP module lets us make HTTP requests. Because it’s a built-in module, we can simply require it and use it. The HTTP module has various methods to handle requests and responses to our Node application. Let’s look at an example:

```jsx
const http = require('http'); // requiring the HTTP module

const server = http.createServer((req, res) => { // creating a server variable
    if (req.url === '/') {
        res.write("This is my first Node.js app");
        res.end(); // we need to end the response once we are done
    } else {
        res.write("Access Denied");
        res.end();
    }
})

server.listen(8080); // setting up a port to run the server on
```

>Pay attention to the **`createServer()`** method we used. It takes in two parameters: **REQ** is for request and **RES** for response. We used an if statement to respond with a message if the request is being made to the home directory ("/"). In case the request is being made to another location other than the home directory, we simply respond with a message saying "Access Denied". 

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
});

server.listen(8080);
```

>Notice how we just used the **`end()`** method instead of using the **`write()`** method?
  This is because we can just end the **HTTP** connection with a message instead of using the **`write()`** method

>**💡 We can also pass in responses as HTML code**

****
## FS Module

The FS module short for **FILE SYSTEM**, let’s us read and write to file on the device. There are two ways of using the File System. The first one is the synchronous method and the other way is asynchronous method. Let’s look at how we can use both of them:

### FS Module Synchronous

This way of using the File System lets us read and write to files in a synchronous (blocking) fashion. Before we start using the FS module, we need to import it since it’s an inbuilt module.

#### Reading from a file
 
```jsx
const { readFileSync } = require('fs'); // using destructuring

const first = readFileSync('./first.txt', 'utf-8');
console.log(first);

const second = readFileSync('./second.txt', 'utf-8');
console.log(second);

```

```scala

FILE CONTENT:

This is the first text
This is the second text

```

>**Notice that we provided `utf-8` as a parameter to the `readFileSync()` function. This is because if we do not provide the file encoding, it would simply return a buffer, which may not be useful for us. Therefore, we need to provide the file encoding type when we read from a file using the FS module.**

****
#### WRITING TO A FILE
```jsx
const { writeFileSync } = require('fs'); // using destructuring

writeFileSync('./write.txt', 'Hello');

```

```scala
FILE CONTENT:

Hello
```
****
#### APPENDING TO A FILE

```jsx
// We can also append to an existing file. To do this, we must use a flag.

const { writeFileSync } = require('fs');

writeFileSync('./write.txt', " I'm Enoch", { flag: 'a' });
// The file already contains the text "Hello" in it; now we are appending to it

console.log('File updated successfully');

```

```scala
FILE CONTENT:

Hello I'm Enoch 
```
****
### FS Module Asynchronous

This way of using the File System lets us read and write to files asynchronously (non-blocking). Using the FS module asynchronously is somewhat different from using it synchronously.

First, the method name is **`readFile()`** instead of **`readFileSync()`**. We pass a callback function into **`readFile()`** with two parameters: “**err**” and “**result**”. The callback function handles any errors that occur when reading the file asynchronously.

> 💡 **We need to provide a callback function to the `readFile()` function because we are dealing with asynchronous code.**

> ⚠️ **Remember that we can access the callback variables only within the callback itself. If we try to access the variable data from outside the callback function block, we will get an error because we cannot read the variables from outside the callback function block.**

#### READING FROM A FILE
```jsx

const { readFile } = require('fs'); // using destructuring

readFile('./first.txt', 'utf-8', (err, result) => {   // passing a callback
    if (err) {
        console.log("An error occurred. Details below:");
        console.log(err);
    } else {  // if no error
        console.log(result); // must be inside the callback function block
    }
});
```

>**Notice the callback function we provided to the `readFile()` function. Also notice that we provided the encoding format which is important**

```scala
FILE CONTENT:

This is the first text
```
****
### WRITING TO A FILE
```jsx

const { writeFile } = require('fs');

writeFile('./first.txt', 'Hello World!', (err) => {
    if (err) {
        console.log(`An error has occurred: ${err}`);
    } else {
        console.log('File written successfully');
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
const { writeFile } = require('fs');

// Create a promise for writing to a file
const write = new Promise((resolve, reject) => {
    writeFile('./first.txt', 'Bye', (err) => {
        if (err) {
            reject(err);
        } else {
            resolve('File written successfully');
        }
    });
});

// Handle the promise
write
    .then((message) => {
        console.log(message);
    })
    .catch((error) => {
        console.log(`An error occurred: ${error}`);
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
const { readFileSync } = require('fs');

// Wrap synchronous file reading in a promise
const read = new Promise((resolve, reject) => {
    try {
        const data = readFileSync('./first.txt', 'utf-8');
        resolve(data);
    } catch (err) {
        reject(err);
    }
});

// Handle the promise
read
    .then((data) => {
        console.log(data);
    })
    .catch(() => {
        console.log('There was an error reading the file');
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

> [!NOTE]
> 1. **Writing Files**: The promise for **`writeFile`** is correctly implemented to handle success and error cases.
> 2. **Reading Files**: The synchronous **`readFileSync`** method is wrapped in a promise to simulate promise-based handling, even though it is inherently blocking. Using **`readFile`** with promises is typically preferred for non-blocking operations.

****
Your content looks well-structured and informative. I've made a few corrections and improvements to enhance clarity and accuracy:

## Using the Node Package Manager (NPM)

The Node Package Manager (npm) is a vast repository of JavaScript packages created by other developers. It allows us to reuse code in our projects, facilitating faster development. NPM simplifies finding, installing, and managing these packages, making it an essential tool for JavaScript developers. You can access NPM at [**www.npmjs.com**](http://www.npmjs.com/).

> **💡 When you install Node.js, NPM is automatically installed along with it.**

### Installing Node Packages

You can install a Node package for two primary purposes: as a local dependency for a specific project or globally to use in any project. Here’s how to do both:

#### As a Local Dependency

```bash
npm install <packageName>
```
#### As a Global Dependency

```bash
npm install -g <packageName>
```

### `package.json` File

The `package.json` file is a manifest that contains crucial information about the project you are building. There are two ways to set up the `package.json` file:

#### Manual Approach

This approach requires manually filling in details like the author’s name, project description, etc.

```bash
npm init
```

#### Default Approach

This approach automatically fills in default details for the `package.json` file, allowing you to start working on your project immediately.

```bash
npm init -y
```

## Event Loops

When dealing with asynchronous JavaScript code, we don’t want the subsequent code to pause and wait for the asynchronous code to finish, as this can slow down the application. The event loop helps manage this by temporarily unloading the asynchronous code and executing the following code first. Once the immediate code is executed and there are no more tasks in the queue, the event loop processes the asynchronous code. This mechanism helps keep the application responsive. Let’s look at an example:

```jsx
setTimeout(() => {
    console.log("I should be displayed first");
}, 0);
console.log("I should be displayed second");
```

```scala
OUTPUT:

I should be displayed second
I should be displayed first
```

> **Notice that even though `setTimeout()` has a delay of 0 milliseconds, the code below it executes first. JavaScript handles asynchronous code using the event loop, regardless of how short the delay is.**

> **The same concept applies to the `setInterval()` function!**

```jsx
setInterval(() => {
    console.log("Hello");
}, 2000);
console.log("Goodbye");
```

```scala
OUTPUT:

Goodbye
Hello
Hello
Hello
Hello
......keeps going // because it's setInterval()
```

### Listening for Events in Node.js

Just as you can listen for events in vanilla JavaScript using the **`addEventListener()`** method, you can listen to events in Node.js using the `events` module. Here’s how you can do it:

```jsx
const EventEmitter = require('events'); // requiring the events module
const event = new EventEmitter(); // creating an instance of EventEmitter

event.on("startCar", () => {
    console.log("Vroom Vroom!");
});

event.emit("startCar"); // triggering the event
```

```scala
OUTPUT:

Vroom Vroom
```

> **Notice that we create a new instance of `EventEmitter` because the `events` module exports a class with various functionalities to work with events. Similar to the `addEventListener()` method in vanilla JavaScript, you can have multiple listeners for the same event type.**

> **You can also pass multiple arguments to the `emit()` method and capture them as parameters in the listener or `on()` method. Here’s an example:**

```jsx
const EventEmitter = require('events');
const event = new EventEmitter();

event.on("startCar", (carManufacturer, carVariant) => {
    console.log("Vroom Vroom!");
    console.log(`The car is built by ${carManufacturer} and the variant is ${carVariant}`);
});

event.emit("startCar", "Lamborghini", "Urus"); // passing multiple arguments
```

```scala
OUTPUT:

Vroom Vroom!
The car is built by Lamborghini and the variant is Urus
```

## Streams Module

The Streams Module in Node.js is used to handle streaming data, allowing you to read or write data sequentially or continuously. This is particularly useful for processing large amounts of data efficiently without loading everything into memory at once.

## Types of Streams in Node.js

In Node.js, there are four main types of streams:

- **Writable**: Used to write data sequentially.
- **Readable**: Used to read data sequentially.
- **Duplex**: Can be used for both reading and writing data sequentially.
- **Transform**: Used when you want to modify the data while it is being read or written.

For this example, we’ll use the **`createReadStream()`** function from the **FS** module.

**Let’s look at some code examples:**

#### Reading from a File with `createReadStream()`

```jsx
const { createReadStream } = require('fs'); // using destructuring

const stream = createReadStream('./first.txt'); // reading from a file

stream.on('data', (chunk) => { // listening to events
    console.log(chunk.toString()); // convert buffer to string for display
});
```

> **In the above code, we use the `createReadStream()` function to read data from a file sequentially. The `on()` method listens for the 'data' event, and the data is displayed using `console.log()`. The `chunk` is a Buffer by default, so we convert it to a string using `chunk.toString()` for readable output.**

#### Buffer Size and `highWaterMark`

A buffer temporarily stores data being transferred or processed in a stream. The default buffer size is **64 KB**. You can control the buffer size using the **`highWaterMark`** option in the `createReadStream()` function.

##### Custom Buffer Size Example

```jsx
const { createReadStream } = require('fs');

const stream = createReadStream('./first.txt', { highWaterMark: 8000 }); // 8 KB buffer

stream.on('data', (chunk) => {
    console.log(chunk.toString()); // convert buffer to string for display
});
```

```scala
OUTPUT:

Hello World 0
Hello World 1
Hello World 2
Hello World 3
Hello World 4
...
```

> **The file being read was `16KB`, so with a custom buffer size of `8KB`, the output is split into two buffers. Each buffer is logged separately.**

> **You can also specify the file encoding format when reading the file by including the `encoding` option in the object passed to `createReadStream()`. For example:**

```jsx
const { createReadStream } = require('fs');

const stream = createReadStream('./first.txt', { encoding: 'utf8' }); // specify encoding

stream.on('data', (chunk) => {
    console.log(chunk); // already a string due to encoding
});
```

---

> **By setting the `encoding` option, the chunks received from the stream are automatically strings, making it easier to work with textual data directly.**

```jsx
const { createReadStream } = require('fs');

const stream = createReadStream('./first.txt', {
    highWaterMark: 8000, // Set buffer size to 8 KB
    encoding: 'utf8'     // Specify encoding to get strings directly
});

stream.on('data', (chunk) => {
    console.log(chunk); // Output as string
});
```

> **The `encoding` option ensures that the chunks received from the stream are automatically strings, making it easier to handle textual data directly.**

### Handling Errors

Since streams use the `EventEmitter` class to handle events, you can also listen for errors that may occur during stream operations. Here’s how you can catch and handle errors using the `on()` method:

```jsx
const { createReadStream } = require('fs');

const read = createReadStream('./first.txt', { // Intentional error for demonstration
    highWaterMark: 8000,
    encoding: 'utf8'
});

read.on('data', (chunk) => {
    console.log(chunk);
});

read.on('error', (err) => { // Handle errors
    console.log("An error has occurred. Details below:");
    console.log(err);
});
```

```scala
OUTPUT:

An error has occurred. Details below:
[Error: ENOENT: no such file or directory, open './first.txt'] {
  errno: -2,
  code: 'ENOENT',
  syscall: 'open',
  path: './first.txt'
}
```

> **In this example, an error occurs because the file path is incorrect. The `error` event handler catches this and logs the error details.**

### Sending Data in Chunks

When dealing with large amounts of data, sending it in chunks is more efficient than sending it all at once. Here's how to use streams to handle data transmission in chunks from a server:

**Example 1: Sending Data All at Once**

In this example, we send the entire content of a file at once, which may not be suitable for very large files.

```jsx
const http = require('http');
const fs = require('fs');

const server = http.createServer((req, res) => {
    fs.readFile('./largeFile.txt', (err, data) => {
        if (err) {
            res.writeHead(500);
            res.end("Error reading file");
            return;
        }
        res.writeHead(200, { 'Content-Type': 'text/plain' });
        res.end(data); // Send all data at once
    });
});

server.listen(3000, () => {
    console.log('Server listening on port 3000');
});
```

**Example 2: Sending Data in Chunks**

In this example, we use streams to send data in chunks, which is more efficient for large files.

```jsx
const http = require('http');
const fs = require('fs');

const server = http.createServer((req, res) => {
    const readStream = fs.createReadStream('./largeFile.txt');
    
    res.writeHead(200, { 'Content-Type': 'text/plain' });
    
    readStream.on('data', (chunk) => {
        res.write(chunk); // Write chunks of data
    });
    
    readStream.on('end', () => {
        res.end(); // End response when all data is sent
    });
    
    readStream.on('error', (err) => {
        res.writeHead(500);
        res.end("Error reading file");
    });
});

server.listen(3000, () => {
    console.log('Server listening on port 3000');
});
```

> **Sending data in chunks is the recommended approach for large files. It prevents excessive memory usage by processing data incrementally, making the server more efficient and responsive.**

---
### Traditional Way of Sending Data from the Server All at Once

```jsx
const http = require('http');
const fs = require('fs');

http.createServer((req, res) => {
    fs.readFile('./first.txt', 'utf-8', (err, data) => {
        if (err) {
            console.log("Oops, some error has occurred!");
            res.writeHead(500, { 'Content-Type': 'text/plain' });
            res.end("Internal Server Error");
        } else {
            res.writeHead(200, { 'Content-Type': 'text/plain' });
            res.end(data); // Ending the connection right away after sending the data
        }
    });
}).listen(8080, () => {
    console.log("Server Started on port 8080...");
});
```

> **In the above code, the entire content of the file is read into memory and then sent in a single response. This method is simple but may not be efficient for very large files due to potential high memory usage.**

### Using the Streams Module to Send Data in Chunks

```jsx
const http = require('http');
const fs = require('fs');

http.createServer((req, res) => {
    const readStream = fs.createReadStream('./first.txt', 'utf-8');
    
    readStream.on('open', () => {
        readStream.pipe(res); // Send the data in chunks
    });
    
    readStream.on('error', (err) => {
        res.writeHead(500, { 'Content-Type': 'text/plain' });
        res.end("Internal Server Error");
    });
}).listen(8080, () => {
    console.log("Server Started on port 8080...");
});
```

> **In this example, using the Streams module allows data to be sent in chunks rather than all at once. The `pipe()` method is used to handle the flow of data from the file to the response object. This approach is more efficient for large files as it minimises memory usage by processing data incrementally.**

---
## HTTP Headers

HTTP headers provide additional information about the response or request. They can specify metadata such as the content type, status codes, and more. By setting custom HTTP headers, you can control how clients interpret your responses.
### Setting Custom HTTP Headers

To set custom HTTP headers, you use the **`writeHead()`** method on the response object. Here’s an example of setting a custom **`Content-Type`** header:

```jsx
const http = require('http');

const server = http.createServer((req, res) => {
    res.writeHead(200, { "Content-Type": "text/html" });
    res.write('<h1>Hello There!</h1>');
    res.end();
});

const serverPort = 8080;
server.listen(serverPort, () => {
    console.log(`Server started on port ${serverPort}`);
});
```

> **Note:** The **`writeHead()`** method is used to set the HTTP status code and headers for the response. In this case, **`200`** indicates a successful request, and **`Content-Type: text/html`** specifies that the response is HTML.

> **💡 In frameworks like Express.js, many of these headers are handled automatically. However, when using the built-in `http` module, you need to set them manually.**

### Putting It All Together

Here’s an example of a simple web server that handles different types of requests and sets appropriate HTTP headers:

```jsx
const http = require('http');
const fs = require('fs');

// Function to handle server-side errors
function resourceUnavailable(res) {
    res.writeHead(503, { "Content-Type": "text/html" });
    res.write("<h1>There was an error. Contact the Server Admin.</h1>");
    res.end();
}

// Function to handle successful responses
function resourceAvailable(res, content) {
    res.writeHead(200, { "Content-Type": "text/html" });
    res.write(content);
    res.end();
}

const server = http.createServer((req, res) => {
    if (req.url === '/') {
        fs.readFile('./html/home.html', (err, content) => {
            if (err) {
                resourceUnavailable(res);
            } else {
                resourceAvailable(res, content);
            }
        });
    } else if (req.url === '/about') {
        fs.readFile('./html/about.html', (err, content) => {
            if (err) {
                resourceUnavailable(res);
            } else {
                resourceAvailable(res, content);
            }
        });
    } else if (req.url === '/contact') {
        fs.readFile('./html/contact.html', (err, content) => {
            if (err) {
                resourceUnavailable(res);
            } else {
                resourceAvailable(res, content);
            }
        });
    } else {
        res.writeHead(404, { "Content-Type": "text/html" });
        res.write("<h1>RESOURCE NOT FOUND!</h1>");
        res.end();
    }
});

const serverPort = 8080;
server.listen(serverPort, () => {
    console.log(`Server started on port ${serverPort}`);
});
```

> **Explanation:**
> - **Error Handling**: The `resourceUnavailable()` function handles server errors by sending a `503 Service Unavailable` response with an appropriate message.
> - **Successful Responses**: The `resourceAvailable()` function sends a `200 OK` response along with the requested file content.
> - **Routing**: Based on the URL, the server reads the corresponding HTML file and sends it back to the client. If the URL does not match any known route, a `404 Not Found` response is sent.

---
## Express.js

Express.js is a minimal and flexible Node.js framework that simplifies the creation of web servers. It builds on the core functionality provided by Node.js, offering a more straightforward and efficient approach to handling HTTP requests and responses.

> ⚠️ **Before using Express.js, you need to install it. Use the command: `npm install express`.**

## Reasons to Use Express.js

- **Routing:** Easily define routes for different URLs and link them to specific handler functions.
- **Middleware:** Use middleware to process requests and responses before they reach your application logic.
- **Templating:** Generate dynamic HTML based on data with integrated templating engines.
- **Error Handling:** Gracefully catch and handle errors.
- **Session Management:** Manage user data between requests with built-in session management.
- **Performance:** Known for its high performance and scalability.
- **Community:** Large, active community for support and resources.
- **Documentation:** Comprehensive and well-written official documentation.

> 💡 **Express.js eliminates the need for the Node.js HTTP module for server creation, providing built-in methods to handle HTTP protocols and responses.**

### Creating a Simple HTTP Server with Express

Here's how you can create a basic HTTP server using Express.js:

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

**Key Points:**

- **No Need for HTTP Module:** Express handles server creation and request/response management, so you don’t need the Node.js `http` module.
- **Using `app.all()`**: This method handles requests for all HTTP methods and can be used to catch any routes not explicitly defined.
- **Using `app.get()`**: This method is used to handle GET requests. The **`send()`** method is used to send responses instead of **`end()`** and **`write()`**.
- **Chaining Methods:** The **`status()`** method is chained with **`send()`** to set the response status code and send a response in a clean, readable manner.

### Serving HTML Files with Express

To serve HTML files as responses, you can use the **`sendFile()`** method along with the `path` module:

```jsx
const express = require('express');
const path = require('path'); // To handle file paths
const app = express();

app.get('/', (req, res) => {
    res.sendFile(path.resolve('./html/home.html'));
    res.status(200);
});

app.get('/about', (req, res) => {
    res.sendFile(path.resolve('./html/about.html'));
    res.status(200);
});

app.all('*', (req, res) => {
    res.status(404).send("<h1>Resource not found!</h1>");
});

const port = 8080;
app.listen(port, () => {
    console.log(`Server started on port ${port}`);
});
```

**Key Points:**

- **Using `path.resolve()`:** This method converts a relative file path to an absolute path, which helps in locating files correctly.
- **Serving Files:** The **`sendFile()`** method sends an HTML file as the response. Ensure the file path is accurate to avoid errors.
- **Status Codes:** Set the status code using **`status()`** before sending the file to ensure the correct HTTP response code is returned.

### Additional Notes

- **Middleware:** Express supports middleware functions that can modify the request or response objects or end the request-response cycle. Middleware can be used for logging, authentication, and more.
- **Routing:** Express allows for more complex routing configurations and supports route parameters and query strings, which can be used to build dynamic routes.

****
# Certainly! Here's the refined content with technical and grammatical corrections, while maintaining the original language, tone, and style:

---

## Static Files

It’s common to have resources like CSS, HTML, and images in our web application. Since these are static and do not change, we can add them to a folder and serve them all at once. This approach is more efficient compared to handling each file individually. Using a dedicated folder for static files keeps the codebase cleaner and more organized.

> 💡 **To serve static files with Express, use the `app.use()` method.**

## Advantages of Storing Static Resources in a Dedicated Folder

1. **Code Organization:** Storing static resources in a dedicated folder promotes better code organization and maintainability, making it easier to locate and manage these files.
2. **Performance Optimization:** Serving static files directly from a dedicated folder improves performance, as the server doesn't need to process each request for these resources.
3. **Caching:** Static files can be cached by browsers and web servers, enhancing performance and reducing server load.
4. **Simplification:** Managing static files separately simplifies routing and handling of dynamic content, such as server-side generated pages or API responses.
5. **Scalability:** As your web application grows, managing static files in a separate folder helps maintain scalability and efficiency.

> ⚠️ **It’s crucial to place the `app.use()` method above all other route-specific methods, such as `app.get()`, `app.post()`, etc. If you place `app.use()` after these methods, it will never be reached.**

### The Bad Approach

<mark style="background: #CACFD9A6;">`index.html`</mark>

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

**Bad Approach of Sending Resources Individually**

```jsx
const express = require('express');
const path = require('path');
const app = express();

// Handling static files individually
app.get('/home.css', (req, res) => {
    res.status(200).sendFile(path.resolve('./static/home.css'));
});

app.get('/cat.png', (req, res) => {
    res.status(200).sendFile(path.resolve('./static/cat.png'));
});

app.get('/', (req, res) => {
    res.status(200).sendFile(path.resolve('./index.html'));
});

app.all('*', (req, res) => {
    res.status(404).send("<h1>Resource not found!</h1>");
});

const port = 8080;
app.listen(port, () => {
    console.log(`Server started on port ${port}`);
});
```

> **In this approach, each static file is served individually. While this may work for small projects, it becomes inefficient as the number of static files grows.**

### Efficient Approach: Serving Resources Using `express.static()`

By using **`express.static()`**, you can serve all static files from a directory with minimal code.

**Directory Structure:**
```
/project
  /static
    /home.css
    /cat.png
  index.html
  server.js
```

**Efficient Approach of Serving Static Files**

```jsx
const express = require('express');
const path = require('path');
const app = express();

// Serve static files from the 'static' directory
app.use(express.static(path.join(__dirname, 'static')));

app.get('/', (req, res) => {
    res.status(200).sendFile(path.resolve('./index.html'));
});

app.all('*', (req, res) => {
    res.status(404).send("<h1>Resource not found!</h1>");
});

const port = 8080;
app.listen(port, () => {
    console.log(`Server started on port ${port}`);
});
```

> **The key here is the `express.static()` middleware, which serves all files in the specified directory. This method handles file paths and status codes, making the code much simpler.**

>⚠️ **Ensure that your main HTML file is named `index.html` so that it is automatically served at the root URL (`/`). This prevents the URL from including `index.html`.**

### Conclusion

Using `express.static()` is the preferred method for serving static files in Express.js. It simplifies file management, improves performance, and keeps your codebase organized.
Here's the refined content on JSON basics with technical and grammatical corrections, while keeping the original tone and language:

---
### JSON Basics

JSON stands for **JavaScript Object Notation**. It is a text format that is completely language-independent, commonly used for transmitting data in web applications. For example, JSON can be used to send data from a server to a client to be displayed on a web page, or vice versa. With Express, you can easily respond to HTTP requests with JSON data. Here’s an example:

```jsx
const express = require('express');
const app = express();

app.get("/", (req, res) => {
  res.json([
    {
      name: 'Enoch',
      age: 21,
      sex: 'Male',
      working: true
    },
    {
      name: 'Annie',
      age: 25,
      sex: 'Female',
      working: false
    }
  ]);
});

const port = 8080;
app.listen(port, () => {
  console.log(`Server started on port ${port}...`);
});
```

> **In the example above, we have two JSON objects with some values, enclosed in an array. The `res.json()` method is used to send the JSON data as the response.**

```scala
OUTPUT:

[{"name":"Enoch","age":21,"sex":"Male","working":true},
 {"name":"Annie","age":25,"sex":"Female","working":false}]
```

****

## Responding with Minimal Data from JSON Objects

Even if you have a lot of data values in your JSON objects, you can choose to respond with only a subset of these values. This approach helps in sending only the necessary data, keeping the response minimal and focused. Here’s how you can achieve this with an example. We’ll use two files: `data.js` to hold the JSON objects with their values and `app.js` to handle requests and responses.

<mark style="background: #CACFD9A6;">`data.js`</mark>

```javascript
const enoch = {
  name: 'Enoch',
  age: 21,
  sex: 'Male',
  working: true
};

const annie = {
  name: 'Annie',
  age: 25,
  sex: 'Female',
  working: false
};

const users = [enoch, annie]; // Storing the objects in an array

module.exports = { users }; // Exporting the "users" array
```

****

<mark style="background: #CACFD9A6;">`app.js`</mark>

```jsx
const express = require('express');
const app = express();
const { users } = require('./data'); // Requiring the exported "users" array

app.get('/test', (req, res) => {
  const data = users.map(({ name, sex, working }) => {
    return { name, sex, working }; // Returning only some of the values
  });
  res.json(data); // Responding with the filtered "data"
});

const port = 8080;
app.listen(port, () => {
  console.log(`Server started on port ${port}`);
});
```

```scala
OUTPUT:

[{"name":"Enoch","sex":"Male","working":true},
 {"name":"Annie","sex":"Female","working":false}]
```

> **In this example, only the `name`, `sex`, and `working` fields are included in the response. The `map()` function is used to filter out the unwanted fields, providing a cleaner and more efficient response.**

---

## Route Parameters

Suppose we have an Express server connected to a database. The server's purpose is to return data based on user queries. For instance, if a user requests information about "X," we should respond with "X's" data. With a small dataset, you could set up individual **`get()`** methods for specific URLs and use the **`find()`** method to retrieve data. Let’s look at an example:

```jsx
const express = require('express');
const app = express();
const { users } = require('./data');

function search(user) {
  return user.name === "Enoch"; // Return the entire Enoch object if user.name is Enoch
}

app.get('/enoch_age', (req, res) => {
  const enoch = users.find(search);
  res.json(enoch.age);
});

const port = 8080;
app.listen(port, () => {
  console.log(`Server started on port ${port}...`);
});
```

<mark style="background: #CACFD9A6;">data.js</mark>

```javascript
const enoch = {
  name: 'Enoch',
  age: 21,
  sex: 'Male',
  working: true
};

const annie = {
  name: 'Annie',
  age: 25,
  sex: 'Female',
  working: false
};

const users = [enoch, annie];
module.exports = { users };
```

> **However, this approach becomes impractical with a large dataset. Writing a separate **`get()`** method for each possible request and using the **`find()`** method to search and send data would be tedious and inefficient. To address this issue, we use **Route Parameters.**

Route Parameters allow us to handle dynamic values in the URL, eliminating the need for static responses. Instead of hardcoding specific names, Route Parameters let us define a flexible route that can handle different requests. Let’s look at an improved example:

```jsx
const express = require('express');
const app = express();
const { users } = require('./data');

// Route for handling GET requests to /:name
app.get('/:name', (req, res) => {
  // Extract the name parameter from the URL
  const { name } = req.params;

  // Find the user whose name matches the parameter
  const data = users.find((user) => user.name === name);

  // If a user is found, send their data as a JSON response
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

> **Notice how we implement route parameters. We use a colon (`:`) to define the parameter in the endpoint. This approach allows us to handle dynamic requests efficiently.**

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

module.exports = { users };
```

---
## Here is a Breakdown of the Code

### Importing and Destructuring Data

- We import an object from a file named `data` using `require`.
- We then destructure the imported object and assign the value of the key "users" to a variable named `users`.
### **Route Parameter and Request Handling**

- We use the `app.get` method to define a route that handles GET requests to the URL path `/:name`.
- This route utilizes a route parameter named `name`.
- Inside the route handler, we destructure the value of the `name` parameter from the `req.params` object.
### **Finding Data and Sending Response**

- We use the `find` method on the `users` array to locate a user whose name matches the `name` parameter value.
- We pass a callback function to `find`, which receives each user object as the parameter `a`.
- Within the callback, we check if the user's name matches the `name` parameter value.
- If a user is found, the `find` method returns that user object, which is stored in the `data` variable.
- Finally, we use the **`res.json()`** method to send the `data` variable as a JSON response to the client.

---
## Query Parameters

Query parameters are key-value pairs appended to the URL after a question mark (`?`). They provide a way to send additional information to the server along with the URL path. The server can then access these parameters and use them to modify its behavior.

> ⚠️ **It's crucial to define a specific route for the query parameters you want to handle. If you don't, Express.js will route the request to the first matching route regardless of the presence or value of the query parameter.**

Let’s understand this with an example:

```jsx
const express = require('express'); 
const app = express();
const { users } = require('./data');

// Define a route for any path with a parameter (/:route) and handle GET requests
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
    // If conversion failed or query parameter was invalid, send a 404 (Not Found) response
    res.status(404).send("No results found");
  }
});

const port = 8080; 
app.listen(port, () => {
  console.log(`Server started on port ${port}...`);
});
```

##### HTTP REQUEST: <http://localhost:8080/route?query=20>

```scala
OUTPUT:

[{"name":"enoch","age":22,"sex":"Male","working":false},
{"name":"annie","age":34,"sex":"Female","working":true},
{"name":"sonny","age":44,"sex":"Male","working":true}]
```

> **Since we set the query value to 20, the response includes all objects where the age is >= 20.**

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
    name: 'siya',
    age: 11,
    sex: 'Female',
    working: false
  }
];

module.exports = { users };
```

> **💡 You can include as many query parameters as needed. Just ensure they are separated by the AND symbol (`&`). Example: [`http://localhost:8080/route?query=20&anotherQuery=randomStuff`](<http://localhost:8080/route?query=20&anotherQuery=randomStuff>)**

****
# Middleware

Imagine we have a web server with two routes: `/home` and `/about`. We have written a lot of logic for the `/home` route, and we want to apply the same logic to the `/about` route as well.

Initially, we might think we can simply copy and paste the logic from the `/home` route to the `/about` route. However, if we had 200 routes and wanted to apply the same logic to all of them, copying and pasting would be inefficient and make the code cluttered.

So, how do we handle this? The solution is to write a function containing the logic and call this function whenever needed. This is where middleware comes into play. Let’s see how to implement this with an example:

**CODE WITHOUT THE USE OF MIDDLEWARE**

```jsx
const express = require('express');
const app = express();

app.get('/home', (req, res) => {
  const method = req.method; // storing the request method
  const url = req.url; // storing the request URL
  const year = new Date().getFullYear(); // storing the current year

  console.log(method, url, year);
  res.send("Connection to the server ended successfully");
});

app.get('/about', (req, res) => {
  const method = req.method;
  const url = req.url;
  const year = new Date().getFullYear();

  console.log(method, url, year);
  res.send("Connection to the server ended successfully");
});

app.get('/contact', (req, res) => {
  const method = req.method;
  const url = req.url;
  const year = new Date().getFullYear();

  console.log(method, url, year);
  res.send("Connection to the server ended successfully");
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

> **In this example, we repeat the same logic across multiple routes (`/about` and `/contact`), which makes the code redundant and inefficient.**

**CODE USING MIDDLEWARE**

```jsx
const express = require('express');
const app = express();

const logic = (req, res, next) => {
  const method = req.method; // storing the request method
  const url = req.url; // storing the request URL
  const year = new Date().getFullYear(); // storing the current year
  console.log(method, url, year);
  next();
};

app.get('/home', logic, (req, res) => {
  res.end("Home page");
});

app.get('/about', logic, (req, res) => {
  res.end("About page");
});

app.get('/contact', logic, (req, res) => {
  res.end("Contact page");
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

> **In the above code example, we defined a function called `logic` to handle the repeated logic. We then apply this function to any route requiring it by passing it as middleware.**

> The `next` parameter is crucial here. After executing the `logic` function, the `next` function allows Express to proceed to the next middleware or route handler. Without it, the request would not continue to the route handler. Calling `next()` at the end of the `logic` function signals to Express that the middleware has completed its task and the route handler can now be executed. This approach keeps the code clean and avoids repetition.

**Okay, our code looks pretty neat and tidy. But let’s clean it up even more. We can do two things to make our code more concise and organized. Let’s explore those next.**

---
## Moving the Logic Function to a Separate File

It’s always a good practice to separate core functions from the main code. By using `module.exports`, we can make the function available for import in other files using the `require()` function.

**CODE AFTER MOVING THE LOGIC FUNCTION TO A SEPARATE FILE**

```jsx
const express = require('express');
const app = express();
const logic = require('./logic'); // Importing the file where our logic is defined

app.get('/home', logic, (req, res) => {
  res.end("Home page");
});

app.get('/about', logic, (req, res) => {
  res.end("About page");
});

app.get('/contact', logic, (req, res) => {
  res.end("Contact page");
});

const port = 8080;

app.listen(port, () => {
  console.log(`Server started on port ${port}...`);
});
```

<mark style="background: #CACFD9A6;">logic.js</mark>

```jsx
const logic = (req, res, next) => {
  const method = req.method; 
  const url = req.url; 
  const year = new Date().getFullYear(); 
  console.log(method, url, year);
  next();
};

module.exports = logic; // Exporting the logic function
```

## Using the `app.use()` Method

While applying middleware functions to specific routes is useful, it can become cumbersome with multiple middlewares. To streamline this, we can use the `app.use()` method, which allows us to specify middleware functions that apply to multiple routes or even all routes.

> 💡 **Placement of `app.use()` is crucial.** If `app.use()` is placed above other route definitions, it will be applied to all requests handled by those routes. 

You can also limit the scope of middleware by specifying a base path. For example, `app.use('/api', logic)` will apply the `logic` middleware only to routes that start with `/api`. This makes the code cleaner and easier to manage. Let’s see an example:

```jsx
const express = require('express');
const app = express();
const logic = require('./logic');

// Apply the logic middleware to all routes starting with /home
app.use('/home', logic);

app.get('/home', (req, res) => {
  res.end("Home page");
});

app.get('/home/enoch', (req, res) => {
  res.end("Enoch's Desktop Files");
});

app.get('/home/enoch/superSecretFolder', (req, res) => {
  res.end("R2V0IGEgbGlmZSE=");
});

const port = 8080;

app.listen(port, () => {
  console.log(`Server started on port ${port}...`);
});
```

> **Notice how we call the `logic` function using `app.use()` at the top of the file.** This setup applies the middleware to all routes that start with `/home`. This approach simplifies the code and ensures that the middleware logic is consistently applied to all relevant routes without repeating the function call.

---

**By separating the logic into a different file and using `app.use()`, we not only keep the code organized but also make it more maintainable and efficient.**

---
# Intro to Other Common HTTP Requests Using Postman

## POST Request

Since we can't perform **`POST`** requests directly through the browser, we'll use a tool called **Postman**. Postman is an API platform designed for developers. While a command-line tool like **`cURL`** could also be used, Postman is beginner-friendly and provides an intuitive interface. Once you install Postman, we can get started.

**`POST`** requests are a popular method supported by the **`HTTP`** protocol. While the **`GET`** method retrieves data from a server, the **`POST`** method allows us to create or send data to a server. In Express.js, we use the **`POST`** method similarly to how we use the **`GET`** method. Let's look at an example:

```jsx
const express = require('express');
const app = express();

app.use(express.urlencoded({ extended: false })); 

const users = [
  { name: 'Enoch', age: '22' },
  { name: 'Ram', age: '14' },
  { name: 'Jeff', age: '56' }
];

app.post('/home', (req, res) => {
  const data = req.body; // Storing the data sent through the POST method
  users.push(data);
  console.log(users);
  res.send("User created!");
});

const port = 8080;

app.listen(port, () => {
  console.log(`Server started on port ${port}...`);
});
```

> In this code, we have an array called **`users`** with some initial values. The **`app.post()`** method defines the route **`/home`**. When data is sent via the **`POST`** method, it is stored in the `data` variable and then pushed to the **`users`** array. We log the updated **`users`** array to the console and send a response saying "**User created!**". The **`req.body`** method is used to access the data sent through the **`POST`** request.

> Note the highlighted code: when sending **`POST`** requests, we must encode the data properly. This is achieved by using **`express.urlencoded({ extended: false })`**, which parses URL-encoded form data. Without this middleware, the server would receive an empty object.

Now that we have covered the basics of handling **`POST`** requests, let’s explore how to send data using **`POST`** requests with Postman.

![[Pasted image 20240728193040.png]]

As shown in the screenshot, we have sent a **`POST`** request to our **`/home`** route, selecting **URL-encoded** format for the data. We've included two key-value pairs: `name` and `age`. If you’ve configured everything correctly and clicked "Send," the **`POST`** request should be processed successfully, and you should see the message "User Created!" in the response.

Next, check your terminal to confirm that the **`users`** array is updated. The data you sent through the **`POST`** request should be visible in the terminal output, reflecting the newly added user.

**Voila!**
![[Pasted image 20240728193112.png]]

****
## PUT REQUEST

**The PUT request allows us to update or create a resource on the server. Let's look at an example:**

```jsx
const express = require('express');
const app = express();

app.use(express.urlencoded({ extended: false }));

let database = [ // An array containing some data
    { userId: 1, userName: 'enoch', userAge: '22' },
    { userId: 2, userName: 'anny', userAge: '28' },
    { userId: 3, userName: 'deepak', userAge: '14' },
    { userId: 4, userName: 'jerry', userAge: '77' },
    { userId: 5, userName: 'hari', userAge: '34' }
];

app.put('/api/people/:id', (req, res) => {
    const { id } = req.params;  // Extracting the id from the URL
    const { name } = req.body; // Extracting the name from the request body

    // Finding if there's a user with the provided id
    const user = database.find(person => person.userId === Number(id));

    if (!user) {
        // If no user found, send a 404 response
        res.status(404).send(`No data found for user with id: "${id}"`);
    } else {
        // Update the userName in the database with the provided name
        user.userName = name;
        // Send the updated database as a JSON response
        res.status(200).json(database);
    }
});

const port = 8081;

app.listen(port, () => {
    console.log(`Server started on port ${port}...`);
});
```

---
### Explanation

- **Setup**: We import `express` and create an instance of the Express application. We use `express.urlencoded({ extended: false })` to parse URL-encoded data in the request body.
- **Database**: An array named `database` simulates a data store with user information.
- **PUT Route**: We define a route `PUT /api/people/:id` where `:id` is a route parameter representing the user ID.
  - **Extract Parameters**: We extract the `id` from the URL and `name` from the request body.
  - **Find User**: We use the `find` method to locate a user with the matching ID.
  - **Update User**: If a user is found, we update their `userName` with the new value and respond with the updated database.
  - **Handle Not Found**: If no user is found, we respond with a 404 status and an error message.

This approach ensures that you can update user information dynamically based on the provided ID and request body.

#### Now let’s use postman to send a PUT request to update the name in the database with the `userId` of 4

![[Pasted image 20240728193207.png]]

> **As you can see in the screenshot, we have successfully update the data that was in the database array. The previous `userName` of `userId`: 4 was `jerry`”**

---
## DELETE REQUEST

**The DELETE request is used to remove a resource from the server. Let’s look at an example:**

```jsx
const express = require('express');
const app = express();

app.use(express.urlencoded({ extended: false }));

const database = [
    { userId: 0, userName: 'enoch', userAge: '22' },
    { userId: 1, userName: 'anny', userAge: '28' },
    { userId: 2, userName: 'deepak', userAge: '14' },
    { userId: 3, userName: 'jerry', userAge: '77' },
    { userId: 4, userName: 'hari', userAge: '34' }
];

app.delete('/api/people/:id', (req, res) => {
    const { id } = req.params;  // Extract the id from the URL

    // Find the index of the user with the provided id
    const index = database.findIndex(person => person.userId === Number(id));

    if (index === -1) {
        // If no user found, send a 404 response
        res.status(404).send(`No data found for user with id: "${id}"`);
    } else {
        // Remove the user from the database array
        database.splice(index, 1);
        // Send the updated database as a JSON response
        res.status(200).json(database);
    }
});

const port = 8081;

app.listen(port, () => {
    console.log(`Server started on port ${port}...`);
});
```

### Explanation

- **Setup**: We import `express` and create an instance of the Express application. **`express.urlencoded({ extended: false })`** is used to parse URL-encoded data in the request body.
- **Database**: An array named `database` simulates a data store with user information.
- **DELETE Route**: We define a route **`DELETE /api/people/:id`** where `:id` is a route parameter representing the user ID.
- **Extract Parameters**: We extract the `id` from the URL parameters.
- **Find Index**: We use the `findIndex` method to locate the index of the user with the matching ID. This method returns the index of the first element that satisfies the provided testing function.
- **Remove User**: If a user is found (index is not -1), we use **`splice`** to remove the user from the **`database`** array at the given index. **`splice(index, 1)`** removes 1 element at the specified index.
- **Handle Not Found**: If no user is found (index is -1), we respond with a 404 status and an error message.

### Key Points
- **`splice(index, 1)`**: The **`splice`** method modifies the array in place. The first argument is the starting index for removal, and the second argument is the number of elements to remove. In this case, it removes one user from the array at the specified index.
- **Error Handling**: It's important to handle cases where the user with the specified ID does not exist to ensure that the client receives appropriate feedback.

This method provides a clean and effective way to manage and delete resources from your server.

#### Let’s look the results from **POSTMAN**

![[Pasted image 20240728193354.png]]

> **The array value in the 0th position was `{userId: 0, userName: 'enoch', userAge: '22' }` but since we gave `0` in the request parameter, this value from removed from the array**

****
# Routes

Routes help us organize our code so that our main JavaScript file is not cluttered and messy. As our routes grow, it becomes difficult to maintain them if they are all in one file. To solve this, we can use routes to separate them into different files, making it much easier to maintain. Let’s take a look at an example.

> Suppose we have two routes for a fruit store: apples and mangoes. We can create a folder named `routes` and inside that, create two files, one for the apple route and the other for the mango route. Let’s look at the code inside each file:

**`apple.js`**

```jsx
const express = require('express');
const router = express.Router();

router.get("/", (req, res) => {
    res.send("Here is your apple");
});

router.post("/", (req, res) => {
    res.send("Your apple has been sent");
});

router.put("/", (req, res) => {
    res.send("Here is your new apple");
});

router.delete("/", (req, res) => {
    res.send("Your apple was thrown away");
});

module.exports = router;
```

**`mango.js`**

```jsx
const express = require('express');
const router = express.Router();

router.get("/", (req, res) => {
    res.send("Here is your mango");
});

router.post("/", (req, res) => {
    res.send("Your mango has been sent");
});

router.put("/", (req, res) => {
    res.send("Here is your new mango");
});

router.delete("/", (req, res) => {
    res.send("Your mango was thrown away");
});

module.exports = router;
```

**`index.js`**

```jsx
const express = require('express');
const app = express();
const appleRoute = require('./routes/apple'); // Importing the apple file
const mangoRoute = require('./routes/mango'); // Importing the mango file

app.use('/apple', appleRoute); 
app.use('/mango', mangoRoute);

const port = 8080;

app.listen(port, () => {
    console.log(`Server started on port ${port}...`);
});
```

> Notice the highlighted code, we use the **`app.use()`** method and pass in the route names and the value that holds the route location of those route names. Notice in the **`mango.js`** and **`apple.js`** files, we have not provided the route paths because we are already providing the paths while importing the file. Also, note that we are not using the usual **`app.get()`** method; instead, we are using **`router.get()`** method as we are using the route concept.

```jsx
OUTPUT FROM VISITING:
<http://localhost:8080/mango>

Here is your mango
```

```jsx
OUTPUT FROM VISITING:
<http://localhost:8080/apple>

Here is your apple
```

By organizing your routes this way, you keep your main application file clean and make your codebase more manageable and maintainable as your application grows.

****
