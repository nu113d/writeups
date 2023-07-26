# idoriot-revenge (Web)
>The idiot who made it, made it so bad that the first version was super easy. It was changed to fix it.

# About this challenge
We are given a link to a php website that includes a login and a register page.
`http://idoriot-revenge.chal.imaginaryctf.org/`


# Solution
Attempting to register at ``/register.php`` with a username and password we immediately get the source code of the current page 
``` 
Welcome, User ID: 123456789
Source Code
<?php

session_start();

// Check if user is logged in
if (!isset($_SESSION['user_id'])) {
    header("Location: login.php");
    exit();
}

// Check if session is expired
if (time() > $_SESSION['expires']) {
    header("Location: logout.php");
    exit();
}

// Display user ID on landing page
echo "Welcome, User ID: " . urlencode($_SESSION['user_id']);

// Get the user for admin
$db = new PDO('sqlite:memory:');
$admin = $db->query('SELECT * FROM users WHERE username = "admin" LIMIT 1')->fetch();

// Check user_id
if (isset($_GET['user_id'])) {
    $user_id = (int) $_GET['user_id'];
    // Check if the user is admin
    if ($user_id == "php" && preg_match("/".$admin['username']."/", $_SESSION['username'])) {
        // Read the flag from flag.txt
        $flag = file_get_contents('/flag.txt');
        echo "<h1>Flag</h1>";
        echo "<p>$flag</p>";
    }
}

// Display the source code for this file
echo "<h1>Source Code</h1>";
highlight_file(__FILE__);
?>
```
and the url now looks like this
`http://idoriot-revenge.chal.imaginaryctf.org/index.php?user_id=123456789`

Interestingly enough we can see the conditions needed to get the flag.
First, the `user_id` GET parameter has to be `php` and the contents of `$_SESSION` array must include the username `admin`. So, we change the url to `http://idoriot-revenge.chal.imaginaryctf.org/index.php?user_id=php`, set the session cookie to `admin` and reload the page

# Flag
ictf{this_ch4lleng3_creator_1s_really_an_idoriot}

