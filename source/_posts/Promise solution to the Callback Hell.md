---
title: Promise--solution to the Callback Hell
date: 2023-12-21 10:12:30
tags:
---

# 

### Step 1, create two Promises: 'readFilePro' and 'writeFilePro'

Take 'readFilePro' as an example, in the 'readFilePro', we pass a file path to the 'file' argument, all the 'readFilePro' function do is to simply create and return a Promise. 



The Promise wraps 'fs.readFile' function, and use its two params 'resolve' and 'reject' to receive the value of 'data' and 'err' respectively.



'resolve' and 'reject' are actually functions. In 'fs.readFile', 

if we get the data successfully, then call the 'resolve(data)' function to receive the 'data' value, use the data later in 'then()' function

else, call the 'reject('Cannot read this file 😢')' function to receive the string param inside, use it later in 'catch()' function

```javascript
const fs = require('fs');
const superagent = require('superagent');

const readFilePro = (file) => {
  return new Promise((resolve, reject) => {
    fs.readFile(file, (err, data) => {
      if (err) reject('Cannot read this file 😢');
      resolve(data);
    });
  });
};

const writeFilePro = (file, data) => {
  return new Promise((resolve, reject) => {
    fs.writeFile(file, data, (err) => {
      if (err) reject('Could not write file 💔');
      resolve('Write file success!');
    });
  });
};

// Chain the promises
readFilePro(`${__dirname}/dog.txt`)
  //use the data that 'resolve(data)' stored
  .then((data) => {
    //return a new Promise
    return superagent.get(`https://dog.ceo/api/breed/${data}/images/random`);
  })
  //call the Promise just created
  .then((res) => {
    console.log(res.body.message);
    //return a new Promise
    return writeFilePro(`${__dirname}/dog-image.txt`, res.body.message);
  })
  //call the Promise just created
  .then((res) => {
    console.log(res);
  })
  //Whenever any of the Promises above have error, the 'catch' function will be called
  .catch((err) => {
    console.log(err);
  });
```

'Pending Promise', when it successfully gets the data, the 'Pending Promise' turns to a 'Resolved Promise'