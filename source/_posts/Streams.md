---
title: Streams
date: 2023-12-21 10:12:30
tags:
---

Before talking about Streams, let's first take File System module in Node.js as an example:

```javascript
const fs = require("fs");
const server = require("http").createServer();
server.on("request", (req, res) => {
  // Solution 1
   fs.readFile("test-file.txt", (err, data) => {
     if (err) console.log(err);
     res.end(data);
   });
```

traditional method like **fs.readFile()**, will load the entire file to the 'data' variable stored into memory, and only after the loading operation finished, it can then send that data. So when the file is big, or tons of requests hit the server, the Node process will quickly run out of resources and quit working, which is really a huge problem.



### Based on the problems above, here comes the 'Stream':

'Stream' is a way to **transmit the data piece by piece(chunk by chunk)**, so we don't need to load the entire file data into memory, take the code below as an example:

```javascript
const fs = require("fs");
const server = require("http").createServer();

server.on("request", (req, res) => {
  // Solution 2: Streams
   const readable = fs.createReadStream("test-file.txt");
  /* When a new piece of data is consumed, emit 'data' event, 
  and can access to that piece of data in the param 'chunk' in the callback function*/
   readable.on("data", chunk => {
     // Write the piece of data into 'response', 'response' is a Writable Stream
     res.write(chunk);
   });
   /* When all the data is read(consumed), emit 'end' event */
   readable.on("end", () => {
     // Print the data onto the client(i.e. browser)
     res.end();
   });
   readable.on("error", err => {
     console.log(err);
     res.statusCode = 500;
     res.end("File not found!");
   });
```

In the code above, we create a variable called 'readable', which is a **'Readable Stream'** object, the 'Readable Stream' object contains two events 'data' and 'end', which is IMPORTANT to achieve Stream.

First, cut the file content into pieces of data, when a new piece of data is consumed, emit 'data' event, which triggers the 'readable.on()', and send one piece of data to the param in the param 'chunk' in the callback function, then use the 'res' variable, which is a **'Writable Stream'** object, to write that 'chunk' into response

Then, when all the data is read(consumed), emit 'end' event, triggers 'res.end()', print the data onto the client(i.e. browser)

#### But, there are still problems unsolved:

The speed of reading file data to 'readable' variable is way faster than write the data to 'res' variable, the speed is unmatch.



### To solve this problem, here comes solution 3: Pipe

```javascript
const fs = require("fs");
const server = require("http").createServer();

server.on("request", (req, res) => {
  // Solution 3
  const readable = fs.createReadStream("test-file.txt");
  // readableStreamSource.pipe(writeableStreamDestination)
  readable.pipe(res);
});

server.listen(8000, "127.0.0.1", () => {
  console.log("Listening...");
});
```