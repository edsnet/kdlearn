---
author: Edson Donisete
categories: [databases, sqlite, php]
---

# Installing the SQLite

***
In this guide I will explain how to :

- Install SQLite
- Check your PHP configuration file for PDO support drivers.
- Create a a small test with php and SQLite in memory.


Lets get started!

## SQLite Installation

Using the terminal write and execute :

```
sudo apt-get install sqlite
```
It will install the SQLite database support files.

Now add the PHP drivers for the sqlite using:

```
sudo aptitude install php5-sqlite
```
Restart the apache service to reload the configuration:

```
sudo service apache2 restart
```

## Chek the PHP Configuration

Create under the Web folder a basic file named "php_info.php". Open this file and include the following:

```
<?php 
  phpinfo();
?>
```
Click on the tab file to get access to the drop-down menu. 
Choose Save ( You may use the shortcut `CTR + s` ) 
and Preview (`CTR + ALT + s `)

On the web page that shows the PHP configuration, look for the Block named **PDO**.
You should see PDO drivers:	mysql and sqlite. 

  If not go back to the terminal and check the installations for any error message.
If you got it right, congratulations and go to the last step! 

## Test the SQLite installation
Create under the Web folder a file named "sqlite_test.php". 
The following code will a create SQLite  database in memory  :
```
<?php  
error_reporting(E_ALL);
try
{
  // pay attention to the ":" at the end...
    $db = new PDO("sqlite::memory:");
    echo "SQLite created in memory.";
  // create a new table in memory 
    $stmt = $db->exec('CREATE TABLE test (id INT, text STRING)'); 
   
  // insert some things into it. 
    $stmt = $db->exec("INSERT INTO test (id, text) VALUES ('1', 'text message')"); 
    $stmt = $db->exec("INSERT INTO test (id, text) VALUES ('2', 'text message 1')"); 

  // make a select 
    $results = $db->query("SELECT * FROM test");  
    $rows = $results->fetchAll(PDO::FETCH_ASSOC);
                 
  // print some results  
    echo "<pre>";
    print_r($rows); 
    echo "</pre>";
}
catch(PDOException $e)
{
    echo "<b>Error:</b> ".$e->getMessage();
}
```
Save and preview like in the previous step.

If necessary to get any aditional debug info :
Go to the file directory(~/Web) and check using the Terminal.
```
php -f ~/Web/sqlite_test.php
```
It may facilitate to debug any error. 

That's it!
