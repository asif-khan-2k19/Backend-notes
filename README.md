# Backend Notes
## These notes are created from the youtube channel [Sheriyans Coding School](https://www.youtube.com/@thesheryianscodingschool) and from the following videos:
- 🚀 [Backend - The EndGame (Part-1)](https://www.youtube.com/watch?v=ZpszSj3ziQk)
- 🚀 [Backend - The EndGame (Part-2)](https://youtu.be/vccm_5gjSkE?list=PLbtI3_MArDOk7J-8hR6CeB5U6bvgRKNNr)

## Topic 1: Express Generator

  - It is used to automatically create all the files and folder structure for the express js project.
  - To use express-generator, run the following commands in the terminal:
    - To globally install express-generator in the system
      
      ```bash
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

- Steps to *setup* Mongo db:
    1. Install community edition of [mongo db](https://www.mongodb.com/try/download/community). It requires internet connection for installation.
    2. Instal mongoose js by writing command in the terminal:
       ```bash
       npm i mongoose
       ``` 
    4. Require and setup connection.
    ::NOTE:: The code side part of Mongo DB is done in the users.js file.
        step 1: Require mongoose
            -- const mongoose = require('mongoose');

        step 2: Create Database
            -- mongoose.connect("mongodb://127.0.0.1:27017/database_name")
            here,
                a. connect() is a function of mongoose. It is used to establish a connection to a MongoDB database.
                b. mongodb://: This indicates the protocol used to connect to MongoDB.
                c. 127.0.0.1: This is the IP address (localhost) where the MongoDB server is running.
                d. 27017: This is the default port number for MongoDB. MongoDB listens for client connections on this port by default.
                e. database_name: This is the name of the specific database to which you want to connect.

    5. Create Schema
        --  const schemaName = mongoose.Schema({
             //   ( information : Data type ) => Format
                username : String,
                name : String,
                age : Number
            })
    6. Create model and export
        -- module.exports = mongoose.model( "collectionName", schemaName )

============= The Final users.js file will contain the following code ====================

    const mongoose = require("mongoose");

    mongoose.connect("mongodb://127.0.0.1:27017/database_name");

    const schemaName = mongoose.Schema({
        username : String,
        name : String,
        age : Number
    })

    module.exports = mongoose.model( "collectionName" , schemaName )

- Steps to *use* Mongo db:
    1. Import userModel from users.js to index.js by writing the following code 
        +---------------------------------------+
        | const userModel = require("./users"); |
        +---------------------------------------+
        below:
        +-----------------------------------+
        | var express = require('express'); |
        | var router = express.Router();    |
        +-----------------------------------+

        The code will look like this: 
        +---------------------------------------+
        | var express = require('express');     |
        | var router = express.Router();        |
        |                                       |
        | const userModel = require("./users"); |
        +---------------------------------------+
        
    2. To create a user in a collection, write the following code below the above code:

        router.get("/create" , async function(req, res){
            const createdUser = await userModel.create({
                username : "Username",
                name : "Your Name",
                age : 10
            });
            res.send(createdUser);
        })

    3. To find all users in a collection, write the following code below the above code:

        router.get("/allusers", async function(req, res){
            const allusers = await userModel.find();
            res.send(allusers);
        })
    
    4. To find single user in a collection, write the following code below the above code:

        router.get("/user", async function(req, res){
            const user = await userModel.findOne({username : "Username"});
            res.send(user);
        })

    5. To delete single user in a collection, write the following code below the above code:
        
        router.get("/delete", async function(req, res){
            const deletedUser = await userModel.findOneAndDelete({username : "Username"});
            res.send(deletedUser);
        })

------------------------------------------------------------------------------------------------------------------------------

Topic 3: Session
    - Session is used to store the data on the server.
    - It can be stored as key-value pairs where keys are unique identifiers and values are the corresponding data.
    - It can be stored for a specific duration or until the browser session ends.
    - Express provides middleware called `session` which allows us to use sessions in our application.
    - Steps To implement Sessions in your app are as follows :
        1. Install the required package by running `npm install express-session`
        2. Require express-session in app.js by using `var session = require("express-session")`
        3. Configure session in middleware using
            app.use(session({
                resave : false,
                saveUninitialized: false,
                secret : "holabolaholabola"
            }));

            here, 
                a. resave: This option controls whether the session should be saved back to the session store if it was not modified    
                    during the request. Setting it to false helps to prevent unnecessary session saves.
                b. saveUninitialized: This option controls whether a session should be stored if it is unmodified (not initialized).
                c. secret: This is the secret used to sign the session ID cookie.

    - Steps to create session are as follows [ You have to write the code below in index.js ]:
        
        1. To create a session, write the code in the following way: 
            
            router.get("/", function(req,res){
                req.session.ban = true;
                res.render("index");
            })

        2. To check a session, write the code in the following way: 

            router.get("/checkban", function (req, res) {
                if (req.session.ban === true) res.send("User Banned");
                else res.send("User is not banned");
            });

        3. To destroy a session, write the code in the following way: 

            router.get("/remove", function (req, res) {
                req.session.destroy(function (err) {
                    if (err) throw Error();
                    res.send("Ban removed");
                });
            });

------------------------------------------------------------------------------------------------------------------------------

Topic 4: Cookies
    - Cookie is used to store the data on the browser.
    - They are very useful when you want to keep some information about the user even after he closes the browser.
    - To create cookie, you have to install `cookie-parser` package. This package is automatically installed by the `express-generator`.
    - Also this package is initialized by the `express-generator` using `app.use(cookieParser());`.
    - Steps to create Cookie are as follows [ You have to write the code below in index.js ]:
        
        1. To create a cookie, write the code in the following way: 
            
            router.get('/', function(req, res){
                res.cookie("age" , 25)
                res.send("Created")
            });

        2. To check a cookie, write the code in the following way: 

            router.get('/find', function(req, res){
                console.log(req.cookies);
                res.send("found")
            });

        3. To clear a cookie, write the code in the following way: 

            router.get('/clear', function(req, res){
                res.clearCookie("age");
                res.send("Cleared")
            });

------------------------------------------------------------------------------------------------------------------------------

Congratulations.... 🥳🎉🎊
You have succesfully learnt express-generator, Mongo DB, Session and Cookies

------------------------------------------------------------------------------------------------------------------------------
==============================================================================================================================
------------------------------------------------------------------------------------------------------------------------------

IN THIS VIDEO:
🚀 Backend - The EndGame (Part-2)
https://youtu.be/vccm_5gjSkE?list=PLbtI3_MArDOk7J-8hR6CeB5U6bvgRKNNr

------------------------------------------------------------------------------------------------------------------------------

Topic 1: Flash Messages
    - Flash messages are used to show error messages on a different route.
    - Normally we can't use data of one route into another route. But with the help of flash messages, we can do so.
    - It can be used only after using express-session.
    - Steps to use Flash messages are as follows: 
        1. Install the package called as connect-flash by using `npm install connect-flash` command.
        2. Require connect-flash package in app.js file.
        3. Install the package called as express-session by using `npm install express-session` command.
        4. Setup express-session by writing the following code 

            app.use(session({
                resave : false,
                saveUninitialized: false,
                secret : "holabolaholabola"
            }));

            below `app.set("view engine", "ejs");`

        5. Now write app.use(flash()); below the above code.
        6. Now the app.js file will look like:

            app.set("view engine", "ejs");

            app.use(session({
                resave : false,
                saveUninitialized: false,
                secret : "holabolaholabola"
            }));

            app.use(flash());

    - You have to create the flash messages in the index.js file.
    - To create a flash message, write `req.flash(key, value)` in the route in which you want to create the flash message.
    - To read a flash message, write `req.flash(key)` in the route in which you want to read the flash message.
    - Example,
    ======== For creation of flash message ========
        router.get('/', function(req, res){
            req.flash("name", "Asif Khan")
            res.render("index")
        })

    ======== For reading of flash message ========
        router.get('/find' , function(req, res){
            console.log(req.flash('name'))
            res.send("Check the console...")
        })

------------------------------------------------------------------------------------------------------------------------------

Topic 2: Intermediate Mongo DB Questions

===== We are using the following schema for solving the questions =====

const UserSchema = mongoose.Schema({
    username: String,
    name: String,
    description: String,
    categories: {
        type: Array,
        default: []
    },
    dateCreated: {
        type : Date,
        default: Date.now()
    }
});

    ========================================================================
                                    QUESTIONS
    ========================================================================
    
    1. How can I perform a case-insensitive search in Mongoose?
    Ans:
        router.get('/findone', async function(req, res){
            var searchname = "name";
            var regex = new RegExp("^" + searchname +"$", 'i')
            const user = await userModel.find({username : regex});
            res.send(user)
        });

        here,
            ^ = denotes how the starting should be
            $ = denotes how the end should be
            ::NOTE:: ^ and $ are used for exact matching of search value.
            
    2. How do I find documents where an array field contains all of a set of values?
    Ans: 
        router.get('/find', async function(req, res){
            const users = await userModel.find({categories : { $all : ['html']}});
            res.send(users)
        });

    3. How can I search for documents with a specific date range in Mongoose?
    Ans: 
        router.get('/find', async function(req, res){
            var date1 = new Date('2023-12-21');
            var date2 = new Date('2023-12-29');

            const users = await userModel.find({dateCreated : {
                $gte: date1,
                $lte: date2
            }});
            res.send(users)
        })
    4. How can I filter documents based on the existence of a field in mongoose?
    Ans: 
        router.get('/find', async function(req, res){
            const users = await userModel.find({categories : { $exists: true }});
            res.send(users)
        });

    - How can I filter documents based on a specific field's length in Mongoose?
    Ans: 
        router.get('/find', async function(req, res){
            let user = await userModel.find({
                $expr : {
                $and : [
                    { $gte : [{ $strLenCP : '$username' }, 0]},
                    { $lte : [{ $strLenCP : '$username' }, 9]}
                ]
                }
            })
            res.send(user)
        })
        In this code snippet, `$strLenCP` is used to get the count of characters (including special characters like emojis).

------------------------------------------------------------------------------------------------------------------------------

Topic 3: Authentication and Authorization
    - You have to install the following packages in order to use authentication and authorization:
        1. passport 
        2. passport-local
        3. passport-local-mongoose
        4. express-session
        5. mongoose

        You can install all these packages by running the command:
        `npm install passport passport-local passport-local-mongoose express-session mongoose`

    - Write the code given below in app.js file after `view engine` and before `logger`.
        app.use(session({
            resave: false,
            saveUninitialized: false,
            secret: "holabolaholabola",
        }));

        app.use(passport.initialize());
        app.use(passport.session());
        passport.serializeUser(usersRouter.serializeUser());
        passport.deserializeUser(usersRouter.deserializeUser());

        ::NOTE:: You have to import passport package where all the packages are imported before writing the above code. 
        [ var passport = require("passport"); ]
    
    - Write the code below in users.js file.
        const mongoose = require("mongoose");
        const plm = require("passport-local-mongoose");

        mongoose.connect("mongodb://127.0.0.1:27017/testing_authentication");

        const UserSchema = mongoose.Schema({
            username: String,
            password : String,
            secret : String
        });

        UserSchema.plugin(plm);

        module.exports = mongoose.model("user", UserSchema);

    - Write the code given below in index.js to implement register, login, logout functionalities.
        var express = require("express");
        var router = express.Router();

        const localStrategy = require('passport-local')

        const userModel = require("./users");
        const passport = require("passport");

        passport.use(new localStrategy(userModel.authenticate()));

        router.get("/", function (req, res) {
        res.render("index");
        });

        router.get("/profile", isLoggedIn , function(req, res){
        res.render("profile");
        })

        router.post('/register', function(req, res){
            var userData = new userModel({
                username: req.body.username,
                secret: req.body.secret,
            });

            userModel.register(userData, req.body.password)
                .then(function(registereduser){
                    passport.authenticate("local")(req, res, function(){
                        res.redirect('/profile');
                    })
                })
        });

        router.post("/login", passport.authenticate("local", {
            successRedirect: "/profile" ,
            failureRedirect: "/"
        }), function(req, res) { });

        router.get("/logout", function(req, res, next){
            req.logout(function(err){
                if(err) { return next(err); }
                res.redirect('/')
            })
        });

        function isLoggedIn(req, res ,next){
            if(req.isAuthenticated()){
                return next();
            }
            res.redirect("/");
        }

        module.exports = router;

    - Write the code given below in index.ejs
        <!DOCTYPE html>
        <html lang="en">
        <head>
            <meta charset="utf-8" />
            <meta name="viewport" content="width=device-width, initial-scale=1" />
            <title>register</title>
            <link
            href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css"
            rel="stylesheet"
            integrity="sha384-T3c6CoIi6uLrA9TneNEoa7RxnatzjcDSCmG1MXxSR1GAsXEV/Dwwykc2MPK8M2HN"
            crossorigin="anonymous"
            />
        </head>
        <body>
            <div class="container mt-5">
            <form action="/register" method="post">
                <h1>Register</h1>
                <div class="mb-3">
                <label for="exampleInputEmail1" class="form-label">username</label>
                <input
                    type="text"
                    name="username"
                    class="form-control"
                    id="exampleInputEmail1"
                    aria-describedby="emailHelp"
                />
                </div>
                <div class="mb-3">
                <label for="exampleInputPassword1" class="form-label">Password</label>
                <input
                    type="password"
                    name="password"
                    class="form-control"
                    id="exampleInputPassword1"
                />
                </div>
                <div class="mb-3">
                <label for="exampleInputSecret" class="form-label">secret</label>
                <input
                    type="text"
                    name="secret"
                    class="form-control"
                    id="exampleInputEmail1"
                    aria-describedby="emailHelp"
                />
                </div>
                <button type="submit" class="btn btn-primary">Submit</button>
            </form>

            <hr />

            <form action="/login" method="post">
                <h1>Login</h1>
                <div class="mb-3">
                <label for="exampleInputEmail1" class="form-label">username</label>
                <input
                    type="text"
                    name="username"
                    class="form-control"
                    id="exampleInputEmail1"
                    aria-describedby="emailHelp"
                />
                </div>
                <div class="mb-3">
                <label for="exampleInputPassword1" class="form-label">Password</label>
                <input
                    type="password"
                    name="password"
                    class="form-control"
                    id="exampleInputPassword1"
                />
                </div>

                <button type="submit" class="btn btn-primary">Submit</button>
            </form>
            </div>

            <script
            src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/js/bootstrap.bundle.min.js"
            integrity="sha384-C6RzsynM9kWDrMNeT87bh95OGNyZPhcTNXj1NW7RuBCsyN/o0jlpcV8Qyq46cDfL"
            crossorigin="anonymous"
            ></script>
        </body>
        </html>

        - Write the code given below in profile.ejs to implement logout functionality.     
            <a href="/logout" class="btn btn-danger">Logout</a>
