# php_auth_example

boilerplate code and docs to get php/mysql auth page up and running

# setup

Now install prerequisites.

```
sudo apt update
sudo apt upgrade 
sudo apt install php8.1-cli
sudo apt install php-mysqli
sudo apt install mysql-server-core-8.0
sudo apt install mysql-server
sudo apt install apache2
sudo apt install php8.1 libapache2-mod-php8.1
```

# configure mysql

Need to set up the mysql user.

```
sudo mysql

```

Then run the following in the SQL command line in the web root (/var/www/html).

Replace PASSWORD with a strong password.

```
ALTER USER 'root'@'localhost' IDENTIFIED BY 'PASSWORD';

CREATE DATABASE demo;
USE demo;

CREATE TABLE users (
    id INT NOT NULL PRIMARY KEY AUTO_INCREMENT,
    username VARCHAR(50) NOT NULL UNIQUE,
    password VARCHAR(255) NOT NULL,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP
);

```

# Set up the rest of the website

Follow the instructions here:
https://www.tutorialrepublic.com/php-tutorial/php-mysql-login-system.php
Step 2: Creating the Config File in the webroot location.

Step 3: Creating the registration form, login page, password reset, welcome page, logout page, etc.

Step 4: Test it.

Now you can populate the "members" area.
Make a new directory in the webroot where the members content will live.
Create an index.php in this folder and provide the following inside before adding 
any html.


```
<?php
// Initialize the session
session_start();

// Check if the user is logged in, if not then redirect him to login page
if(!isset($_SESSION["loggedin"]) || $_SESSION["loggedin"] !== true){
    header("location: ../login.php");
    exit;
}
?>
```

The login.php script should point to the member content folder location where 
index.php will serve member content.
The contents of the this folder cannot be viewed without logging in.

Instead of directing all users to the one welcome page, each individual
can have their own directory with their own personalised info.
The following script will direct the user to a folder by the same name.
There needs to be a separate script to create the user's drectory and populate
the content.

```
// Check if the user is already logged in, if yes then redirect him to welcome page
if(isset($_SESSION["loggedin"]) && $_SESSION["loggedin"] === true){

    $username = trim($_POST["username"]);
    $location = "members/" . $username;
    header("location: $location");
    exit;
}
```
