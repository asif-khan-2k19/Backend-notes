# Backend Notes
## These notes are created from the youtube channel [Sheriyans Coding School](https://www.youtube.com/@thesheryianscodingschool) and from the following videos:
- 🚀 [Backend - The EndGame (Part-1)](https://www.youtube.com/watch?v=ZpszSj3ziQk)
- 🚀 [Backend - The EndGame (Part-2)](https://youtu.be/vccm_5gjSkE?list=PLbtI3_MArDOk7J-8hR6CeB5U6bvgRKNNr)

## Topic 1: Express Generator

  - It is used to automatically create all the files and folder structure for the express js project.
  - To use express-generator, run the following commands in the terminal:
    - To globally install express-generator in the system
      
      ```node
      npm i express-generator -g
      ```
    - To create a project with express generator
      ```bash
      express appname --view=ejs
      ```
    - To go to the project directory
      ```bash
      cd appname
      ```
    - To install the node modules in the project
      ```bash
      npm install
      ```
    - Run only if nodemon is not installed
      ```bash
      npm i nodemon
      ```
    - To run the server
      ```bash
      npx nodemon
      ```
    - **NOTE: Do not provide filename while using express-generator. Example, `npx nodemon script.js`**
    
  - The changes that occurred because of using express-generator are:
    - `app.get` is converted to `router.get` in `index.js` and `user.js`
    - `npx nodemon script.js` is converted to `npx nodemon`

------------------------------------------------------------------------------------------------------------------------------

## Topic 2: Mongo db
- It is used to create database for a project.
- The following are the things you should keep in mind while understanding mongo db.

    | **Code side**    | **MongoDB side**  |
    |------------------|-------------------|
    | 1. DB Setup      | DB Formation      |
    | 2. Model         | Collection        |
    | 3. Schema        | Documents         |

- Lets understand these terms:
    - **DB Setup:** *Creation of database*
    - **Model:** *It is the code written to create a collection.*
    - **Collection:** *A collection is a specific section of the database wherea a particular data is stored. Example, user collection - here all the data of different users will be stored.* 
    - **Schema:** *It is the code written to define the structure of a document. It specifies the information to be stored in the collection and its data type.*
    - **Document:** *A document is the single entity of the collection. Example, information about a single user present in the users collection.*

### Steps to *setup* Mongo db:
  - Install community edition of [mongo db](https://www.mongodb.com/try/download/community). It requires internet connection for installation.
  - Instal mongoose js by writing command in the terminal:

    ```bash
     npm i mongoose
     ``` 
  - Follow the steps and write the code in the users.js file to setup connection.
    - Step 1: Require mongoose

      ```js
      const mongoose = require('mongoose');
      ```
    - Step 2: Create Database
   
      ```js
      mongoose.connect("mongodb://127.0.0.1:27017/database_name")
      ```

      here,
        - `connect()` is a function of mongoose. It is used to establish a connection to a MongoDB database.
        - `mongodb://:` indicates the protocol used to connect to MongoDB.
        - `127.0.0.1:` is the IP address (localhost) where the MongoDB server is running.
        - `27017:` is the default port number for MongoDB. MongoDB listens for client connections on this port by default.
        - `database_name:` is the name of the specific database to which you want to connect.

    - Step 3: Create Schema

      ```javascript
      const schemaName = mongoose.Schema({
       //   ( information : Data type ) => Format
          username : String,
          name : String,
          age : Number
      })
      ```
    - Step 4: Create model and export
   
      ```javascript
      module.exports = mongoose.model( "collectionName", schemaName )
      ```

  - After performing all the steps, the `users.js` file will look like.

    ```javascript
    const mongoose = require("mongoose");

    mongoose.connect("mongodb://127.0.0.1:27017/database_name");

    const schemaName = mongoose.Schema({
        username : String,
        name : String,
        age : Number
    })

    module.exports = mongoose.model( "collectionName" , schemaName )
    ```
### Steps to *use* Mongo db:
  - Import `express` package and `userModel` from `users.js` to `index.js`.
    
    ```js
    var express = require('express');
    var router = express.Router();
    
    const userModel = require("./users");
    ```

  - To create a user in a collection, write the following code below the above code:
    ```js
    router.get("/create" , async function(req, res){
        const createdUser = await userModel.create({
            username : "Username",
            name : "Your Name",
            age : 10
        });
        res.send(createdUser);
    })
    ```
  - To find all users in a collection, write the following code below the above code:

    ```js
    router.get("/allusers", async function(req, res){
        const allusers = await userModel.find();
        res.send(allusers);
    })
    ```
  - To find single user in a collection, write the following code below the above code:

    ```js
    router.get("/user", async function(req, res){
        const user = await userModel.findOne({username : "Username"});
        res.send(user);
    })
    ```
  - To delete single user in a collection, write the following code below the above code:

    ```js
    router.get("/delete", async function(req, res){
        const deletedUser = await userModel.findOneAndDelete({username : "Username"});
        res.send(deletedUser);
    })
    ```
------------------------------------------------------------------------------------------------------------------------------

## Topic 3: Session
  - Session is used to store the data on the server.
  - It can be stored as key-value pairs where `keys` are unique identifiers and `values` are the corresponding data.
  - It can be stored for a specific duration or until the browser session ends.
  - Express provides middleware called `session` which allows us to use sessions in our application.
  - Steps To implement `Sessions` in your app are as follows :
    - Install the required package.

      ```js
      npm install express-session
      ```
    - Require express-session in app.js by using
      ```js
      var session = require("express-session")
      ```
    - Configure session in middleware using
      ```js
      app.use(session({
          resave : false,
          saveUninitialized: false,
          secret : "holabolaholabola"
      }));
      ```
      here, 
        - **resave:** This option controls whether the session should be saved back to the session store if it was not modified during the request. Setting it to false helps to prevent unnecessary session saves.
        - **saveUninitialized:** This option controls whether a session should be stored if it is unmodified (not initialized).
        - **secret:** This is the secret used to sign the session ID cookie.

  - Steps to create a `session`. Write the below code in index.js
    - To create a session, write the code in the following way: 

      ```js
      router.get("/", function(req,res){
          req.session.ban = true;
          res.render("index");
      })
      ```
    - To check a session, write the code in the following way: 

      ```js
      router.get("/checkban", function (req, res) {
          if (req.session.ban === true) res.send("User Banned");
          else res.send("User is not banned");
      });
      ```
    - To destroy a session, write the code in the following way: 

      ```js
      router.get("/remove", function (req, res) {
          req.session.destroy(function (err) {
              if (err) throw Error();
              res.send("Ban removed");
          });
      });
      ```

------------------------------------------------------------------------------------------------------------------------------

## Topic 4: Cookies
  - Cookie is used to store the data on the browser.
  - They are very useful when you want to keep some information about the user even after he closes the browser.
  - To create cookie, you have to install `cookie-parser` package. This package is automatically installed by the `express-generator`.
  - Also this package is initialized by the `express-generator` using `app.use(cookieParser());`.
  - Steps to create `Cookie`. Write the below code in index.js

    - To create a cookie, write the code in the following way: 
    
      ```js          
      router.get('/', function(req, res){
          res.cookie("age" , 25)
          res.send("Created")
      });
      ```

    - To check a cookie, write the code in the following way: 
    
      ```js
      router.get('/find', function(req, res){
          console.log(req.cookies);
          res.send("found")
      });
      ```

    - To clear a cookie, write the code in the following way: 
    
      ```js
      router.get('/clear', function(req, res){
          res.clearCookie("age");
          res.send("Cleared")
          });
      ```

------------------------------------------------------------------------------------------------------------------------------

# Congratulations.... 🥳🎉🎊
## You have succesfully learnt express-generator, Mongo DB, Session and Cookies
