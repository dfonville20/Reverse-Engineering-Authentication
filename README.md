# Reverse-Engineering-Authentication

User Story:
The user can log into a site or app safely by using their email and password, which then are stored in the MySQL database inside a user table.

Reverse Engineering:

Folder > Config >
Contains files: config.json, passport.js, isAuntheticated.js

Config.json is the package that contains the database info, host address, as well as user and password credentials for when user connects to database.

Passport.js → Imports functions “Passport’ and “Local Strategy” prompting the user to “login” by entering their email(as opposed to username/user) and password. Then passport.js will pull database(db) from the “models” folder to see if what has been entered matches with email/password stored in the database. If either are incorrect, then user will be returned to main(root) page via isAuthenticated.js which is the middleware. The same middleware will be used to redirect user to the restricted route if both username and password are both correct. The passport is then serialized/deserialized to keep the user authenticated throughout all the HTML’s when they are browsing through pages on the site so they will not have to log in each time. The function “Passport” is then exported to be used in other files.

Folder → Models
Contains files: index.js and user.js

Index.js handles the starting, routing, and functions of this application. It declares what other modules are required for functionality of the application.

var fs = require('fs');
var path = require('path');
var Sequelize = require('sequelize');
var basename = path.basename(module.filename);
var env = process.env.NODE_ENV || 'development';
var config = require(\_\_dirname + '/../config/config.json')[env];
var db = {};

Are all the modules being required for function. It imports the config.json package and environmental variables(env) to use as well as the Sequelize constructor. It then establishes the route paths before exporting db to be used in other files (passport.js).
User.js imports bcrypt (bcryptjs) to convert password into hashes. It allows for creation of a “new user”.

module.exports = function(sequelize, DataTypes) {

Exports the functions “sequelize” and “DataTypes” to be used in other files.

email: {
type: DataTypes.STRING,
allowNull: false,
unique: true,
validate: {
isEmail: true
}
},

The user is to enter their email (DataTypes.STRING) which cannot be left blank (allowNull: false) the email must be unique. Email address will be checked in database to see if there are any matching emails. If email is unique then it will be validated.

    password: {
      type: DataTypes.STRING,
      allowNull: false

User then enters their own password (DataTypes.STRING). Password cannot be left blank (allowNull: false) Password will then be created and hashed.

Folder → Public
Contains folders: “js” and “stylesheets” and files: “login.html” “members.html” and “signup.html”

Login.html → root or main HTML, user logs in here. Using the script from login.js if login info is correct, user will be redirected to “members.html” which uses the members.js. If login is unsuccessful then user will be directed to “signup.html”

Folder →Public --> stylesheets
Contains file: style.css
Is the styling for the HTML.

Folder → Public → JS
Contains files: “login.js” “members.js” “signup.js”

Login.js creates the loginForm that prompts a submit event when user puts in their email (emailInput) and password(passwordInput). The form is then cleared and if login is correct, user is redirected to members.html.

Members.js does a GET request to figure out which users are logged in and keeps the HTML up-to-date.

Signup.js creates a signUpForm where user enters email (emailInput) and password(passwordInput). Upon submit event, forms will be validated as not left blank, and user will be taken to member page. Signup.js is the running script used in signup.html.

Folder → Routes
Contains files: “api-routes.js” “html-routes.js”

Api-routes.js imports db and passport that can be used for the four different routes.
The first route connects to a login/api that uses passport.authenticate middleware to validate the login credentials and if everything matches, will direct user to members.html.

The next route connects to signup/api where the user to enter a unique email and password to be used. The password will be hashed by users.js and store securely in db. Once user is signed up they will be redirected to members.html.

The next route is simply a logout route.

The last route connects to user_data/api and is used clientside to get user data.

Html-routes.js imports the paths so the routes can be used for the HTML files. Uses isAuthenticated to keep validated users “logged in” when browsing through pages. If a member is not authenticated and tries to log in they will be redirected to signup.html.

File → Server.js
Server.js is the first connecting hub for the app to work. It imports all the packages and functions and declares the PORT. It also contains the middleware boiler plate, and sets the appropriate routes.

File → Package.json
Shows all packages and dependencies that are currently installed in the app. Also what versions are being used.

File → travis.yml
Testing environment for the app.

File → eslintignore
Files to ignore for the purpose of linting.

File → eslintignore.json
Json packages to ignore for the purpose of linting.

File → gitignore
Tells git which files to ignore.

Changes:

• Allow support for multiple users
• Updates to CSS/HTML
• Modernization of layout
• Incorrect password will not erase username
• Link pages
• Fix other broken links
• Add quick links
• Encompass JavaScript tables
