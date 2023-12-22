---
title: Node.js Dependencies
date: 2023-12-21 10:12:30
tags:
---

1. ### nodemon(dev dependency): use nodemon to restart the project automatically when code changed

   #### There are two ways of using nodemon:

   1. Globally(**Recommend for dev depencencies**): use command line 'nodemon ./index.js' instead of 'node ./index.js'

   2. Locally: in package.json-->"scripts", create a new key:value, 

      here I create "placeholder": "nodemon ./index.js", 

      then use command line **'npm run placeholder' or  'npm placeholder'** to start it

```json
package.json
{
  "name": "node-farm",
  "version": "1.0.0",
  "description": "Learning node.js",
  "main": "index.js",
  "scripts": {
    "placeholder": "nodemon ./index.js"
  },
  "author": "Zirao Cheng",
  "license": "ISC",
  "dependencies": {
    "slugify": "^1.6.6"
  },
  "devDependencies": {
    "nodemon": "^3.0.1"
  }
}

```

# Where it stored?

Local dependencies are stored right at the project, in a name of 'node_modules' folder, **btw, never to upload the 'node_modules' folder to github or move it to another computer if you try to run your project on other computers, cuz the dependencies in the 'node_modules' folder can just simply download by npm!**

### if you want to recover the 'node_modules' folder in a new computer:

​	use command line **'npm install'**, then npm will read the 'package.json' in the project, read all 	dependencies and download everything

![image-20230929163240996](C:\Users\orang_9qy7n8e\AppData\Roaming\Typora\typora-user-images\image-20230929163240996.png)

### package-lock.json:

​	Stores ***all the dependencies and all the version*** we're actually using in the project