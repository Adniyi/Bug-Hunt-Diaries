---
date: '2025-05-14T10:39:53-07:00'
draft: false
title: 'How to Create an Api With Php'
tags: ["PHP", "APIs", "Databases"]
categories : ["Tutorial", "APIs"]
---

In this tutorial, I will teach you how to build a simple REST API in what has been called a "dead language." That's right, I’ll be guiding you on how to build a simple API in PHP. The API in this tutorial is inspired by the one I built recently for a project. You can scale this API to meet your needs and make it more robust if you need to.

## 🏢 What are we building
We will be building an API for a simple task management system with fields such as:
- title
- description 
- due_date 
- status

## What to expect 
What you'll get from this tutorial is:
- How to create a simple REST API with vanilla PHP.
- How to write a class to represent your API data.
- How to use PHP's built-in functions to interact with your database.
- How to use MySQL and Apache server to serve our API.

## 🎯 Prerequisites
To get the most out of this tutorial, it’s advised that you know the basics of PHP (obviously), so things like:
1. functions 
2. objects and, 
3. The SUPER GLOBAL variables, you know, the works. 

If you don't know anything about PHP 🤔, then… what in the world are you doing here? 😒 Please 🙏 leave, or you can check out another one of my blogs 😁.

The tools i will be using include:
1. **XAMPP**: You will need to have XAMPP installed. We will be using XAMPP's Apache server to serve our PHP.
2. **A Text-editor**:  I will be using VSCode, but you're free to use any editor of your choice.
3. **An API tester**: I will be using Postman, but alternatives include: VSCode's Thunder-Bolt, Insomnia, etc.

## 📚 Essential Files for this tutorial 
To begin, be sure to set up a directory in your localhost htdocs that will contain all the necessary files. Let’s assume it’s called “API.”

1. **config.php**: This file contains the database configurations and will be used to initialize the database connection.
2. **Task.php**: This file contains the class that will represent the Task with methods for creating, updating, and deleting tasks.
3. **Errorhandler.php**: This contains the generic constructor class for formatting database errors.
4. **index.php**:  This file will serve as the API entry point.


## Creating the database 
Before we get into writing code for the API, launch XAMPP and start your Apache server and MySQL database. Go to localhost in your browser and navigate to PHP ADMIN, then create the database and the required tables for the database.

## config.php
Inside your **config.php** file create yur database class that will take in the database credentials and establish your database connection,

```php
class DatabaseConfig {
    public $hostname;
    public $username;
    public $password;
    public $database;

    public function __construct($hostname, $username, $password, $database){
        $this->hostname = $hostname;
        $this->username = $username;
        $this->password = $password;
        $this->database = $database;
    }

    public function getconnection(){
        $conn = mysqli_connect($this->hostname, $this->username, $this->password, $this->database); 
        if (!$conn) {
            die('Connection Faild: '. mysqli_connect_error());
        }
        return $conn;
    }
}
```
The class has a constructor that takes the database connection credentials, and then we use the getConnection() function to establish the database connection. It checks if the connection has been made and returns the connection object; otherwise, it returns an error.

## Task.php

Inside your Task.php file, create a class that will represent the Task with methods for creating, updating, and deleting tasks. This class serves as the gateway class, a common design pattern in API development.

```php

class Task {
    private $conn;

    public function __construct($conn){
        $this->conn = $conn;
    }

    public function getAllTask(){
        $query = "SELECT * FROM task";
        $result = mysqli_query($this->conn, $query);
        $tasks = [];
        if($result){
            while($row = mysqli_fetch_array($result)){
                $tasks[] = [
                    'id' => $row['id'],
                    'title' => $row['title'],
                    'description' => $row['description'],
                    'due_date' => $row['due_date'],
                    'status' => $row['status']
                ];
            }
            return $tasks;
        }else{
            $data = [
                'status' => 404,
                'message' =>'No Task available'
            ];
            return $data;
        }
    }

    public function getTaskbyId($id){
        $query = "SELECT * FROM task WHERE id = ?";
        $stmt = $this->conn->prepare($query);
        $stmt->bind_param("i", $id); // i = integer
        $stmt->execute();
        $result = $stmt->get_result();
        if($result){
            $task = mysqli_fetch_assoc($result);
            return $task;
        }else{
            $data = [
                'status' => 404,
                'message' =>'No Task with id $id available'
            ];
            echo json_encode( $data );
            return $data;
        }
    }

    public function addTask($task){
        $title = $task["title"];
        $description = $task["description"];
        $due_date = $task["due_date"];
        $status = $task["status"];
        $query = "INSERT INTO task (title, description, due_date, status) VALUES ('$title', '$description','$due_date','$status')";
        $result = mysqli_query($this->conn, $query);
        if($result){
            return true;
        }else{
            return false;
        }
    }

    public function updateTask($id, $task){
        $title = $task["title"];
        $description = $task["description"];
        $status = $task["status"];
        $query = "UPDATE task SET title = '$title', description = '$description', status = '$status' WHERE id = '$id'";
        $result = mysqli_query($this->conn, $query);
        if($result){
            return true;
        }else{
            return false;
        }
    }

    public function deleteTask($id):bool{
        $query = "DELETE FROM task WHERE id = $id";
        $result = mysqli_query($this->conn, $query);
        if($result){
            return true;
        }else{
            return false;
        }
    }
}
```
This class has methods to perform CRUD operations on a task table in the database. The `getAllTasks` method returns all tasks, `getTaskById` returns a task by its ID, `addTask` adds a new task, `updateTask` updates an existing task, and `deleteTask` deletes a task.

The `getAllTask` method returns a JSON encoded array of all tasks. The `getTaskById` method returns a JSON encoded array of a task with the given id. If no task with the given id is passed , it returns a JSON encoded array with a 404 status code and a message indicating that no task.


## Errorhandler.php

When interacting with a database, you may encounter errors such as not being able to connect to the database or not being able to execute a query. This class handles those errors and formats them to fit the API response.

```php
class ErrorHandler {
    public static function handleException(Throwable $exception){

        http_response_code(500);
        echo json_encode([
            'code' => $exception->getCode(),
            "error"=> $exception->getMessage(),
            'file'=> $exception->getFile(),
            'lineNumber'=> $exception->getLine() 
        ]);
    }
}
```
Inside our class, we’ve got a static function called `handleException`.
```php
public static function handleException(Throwable $exception)
```
What’s “static”? It means you don’t have to create an object to use it. It’s like calling someone directly instead of going through a receptionist.

It takes one argument: `$exception`. That’s any error or exception that PHP throws during runtime. It’s wrapped in the `Throwable` type so it can catch both `Exception` and `Error` classes—very inclusive!

`Throwable`  is the base interface for any object that can be thrown via a throw statement, including Error and Exception

This function sets the HTTP response code to 500:
```php
http_response_code(500);
```
That tells the client: “Hey, something’s broken on the server.” It’s the digital version of a server raising its hand and saying, “Oops.”

The class sends back a JSON response with the error details:

```php
echo json_encode([
    'code' => $exception->getCode(),
    'error'=> $exception->getMessage(),
    'file'=> $exception->getFile(),
    'lineNumber'=> $exception->getLine()
]);
```
The `json_encode()` function converts the array into a JSON string. The array contains the following information

## index.php
Now comes the part you've been waiting for: the API endpoint. This file serves as the gateway to effectively handle requests coming to the API. With robust functionality, it acts as a central hub that facilitates seamless communication and data exchange between our application and the external world.

```php
<?php
require_once('config.php');
require_once 'Task.php';
require_once 'Errorhandler.php';



# Construct the object instance
$database = new DatabaseConfig(hostname:'localhost', username:'root', password:'', database:'task');

// Establish the connection
$conn = $database->getconnection();


// create a task object instance
$task = new Task($conn);
set_exception_handler('ErrorHandler::handleException');
header('Access-Control-Allow-Origin:*');
header('Content-type: application/json; charset=UTF-8');
// header('Access-Control-Allow-Origin:*');
header('Access-Control-Allow-Origin:*');
header('Access-Control-Allow-Methods: POST, GET, OPTIONS, PUT, DELETE');

// Get the request method sent to the server
$method = $_SERVER['REQUEST_METHOD'];

// Get the request body
$endpoint = isset($_SERVER['PATH_INFO']) ? $_SERVER['PATH_INFO'] : $_SERVER['REQUEST_URI'];

// Switch statement to handle the different request types
switch ($method) {
    case 'GET':
        if($endpoint === '/tasks') {
            // Get all tasks
            $data = $task->getAllTask();
            header('HTTP/1.0 200 OK');
            echo json_encode($data);
        }else if(preg_match('/^\/tasks\/(\d+)$/', $endpoint, $matches)) {
            // Get task by id 
            $taskid = $matches[1];
            $data = $task->getTaskbyId($taskid);
            if($data != null) {
                header('HTTP/1.0 200 OK');
                echo json_encode($data);
            }else{
                $data = [
                    'status' => 404,
                    'message' =>"No Task with id $taskid available"
                ];
                header("HTTP/1.0 404 No Task with id $taskid available");
                echo json_encode($data);
            }
        }
        break;
    case 'POST':
        if($endpoint === '/tasks') {
            // Create a Task
            $data = json_decode(file_get_contents('php://input'), true);
            $result = $task->addTask($data);
            header('HTTP/1.0 200 OK');
            if($result === true){
                echo json_encode(['success' => $result]);
            }else{
                echo json_encode(['error' => $result]);
            }
        }
        break;
    case 'PUT':
        if(preg_match('/^\/tasks\/(\d+)$/', $endpoint, $matches)) {
            // Update a task
            $taskid = $matches[1];
            $data = json_decode(file_get_contents('php://input'), true);
            $result = $task->updateTask($taskid, $data);
            header('HTTP/1.0 200 OK');
            echo json_encode(['success' => $result]);
        }
        break;
    case 'DELETE':
        if(preg_match('/^\/tasks\/(\d+)$/', $endpoint, $matches)) {
            // Delete a task
            $taskid = $matches[1];
            $result = $task->deleteTask($taskid);
            header('HTTP/1.0 200 OK');
            echo json_encode(['success' => $result]);
        }
        break;
    default:
        header('HTTP/1.0 405 Method Not Allowed');
        echo json_encode(['error'=> 'Method Not Allowed']);
        break;
}
```
##### Explanation:
First we import all the other files using the `require_once` function. Then we create an instance of the database class and establish the database connection in the `$conn` variable.

We then create an instance of the task class and pass the database connection to the class, this will allow us to use the different methods available to the class for manipulating the database.

We use `set_exception_handler` to handle any exceptins or errors that my occure when using the API, it is important to set this as early as possible in the script.

We set the necessary `HTTP` headers, we get the request `method` and the `endpoint` of the request using the `$_SERVER` superglobal.

A `switch` statement is used to determine which method to use based on the request method.

If it is a `GET`, we check if the endpoint is `/tasks` and if so we call the `getAllTask` method of the task class and return the result else if not, we check using the `preg_match` to check if the endpoint is `/tasks/{id}` and if so we call the `getTaskbyId` method passing the `id` recevied from the request.

If the request is a `POST`, we check if the endpoint is `/tasks` and if so we call the `addTask` method.

If the request is a `PUT`, we check if the endpoint is `/tasks/{id}` and if so we call the `updateTask` method.

If the request is a `DELETE`, we check if the endpoint is `/tasks/{id}` and if so we call the `deleteTask` method.

If none of the conditions are true, we use the `default` case to return a `405 Method Not Allowed` status code. and t hen break the switch statement.

## Testing 
You can now test the API by using a tool like Postman or cURL. and see the responses from the API.

**Note**: If you followed my exact steps, when testing the API, your endpoint should be:

```bash
http://localhost/<your-directory>/index.php/tasks
```

## Conclusion
And thats it, congrats on creating a simple RESTful API using PHP and MySQL. This is a basic example and you can improve it by adding more features, error handling, and testing. 

Hope you enjoyed this tutorial, and i'll see you around in another Blog.

## 👋 By.