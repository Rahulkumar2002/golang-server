>### In this Project we will be creating a beginner friendly Server in Golang.  

To create it first we have to create our files and folder in our Code Editor , I'm using VSCode . VSCode is a Open Source code editor from Microsoft & it is quite awesome . 

We will create a **main.go** file and a **static folder** inside this **static folder** , we will create a file named as **index.html** and **form.html**. 

Inside index.html we will write some **HTML** code . Which will only show a text **Static Website** on our web page . Simillary inside **form.html**  , we will create a form with two inputs , first input will be for name and second will be for address of the user . In our **main.go** we will write logic which will serve us file according to the end point hitted in the url . 

Let start building this project with our **index.html** file : 
    To get a basic html syntax like this just press <kbd>shift</kbd> + <kbd>1</kbd> a popup will show with and *Emmet Abbreviation* in your vscode then press enter. Simillar type of code will be given to you .

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    
</body>
</html>
 ```

#### In this we have simply created a static site which only shows two words **Static Website**. 

```html

 <!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Static Website</title>
</head>

<body>
    <h2>Static Website</h2>
</body>

</html>

```

#### In **form.html** we will create a form with  `method = "POST"` , so it can make post request with input data in it . 

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Form Page</title>
</head>

<body>
    <div>
        <form method="POST" action="/form">
            <label for="name1">Name</label><input type="text" name="name" id="name1" value="" />
            <label for="address1">Address</label><input type="text" name="address" id="address1" value="" />

            <input type="submit" value="submit" />
        </form>
    </div>
</body>

</html>


```
There is three input type , first one for name and second one for address of the user.

Let move ahead and make our logic for this server which is our **main.go** file  . So what will be structure for this file . 


<!-- link for structure of image  -->
 ![Struture](image\structImg.png)

As you can see in our diagram , if we we hit <kbd>/</kbd> endpoint our server will serve us **index.html** file . If we hit <kbd>/form.html</kbd> it will serve us **form.html** file  and similarly if we hit <kbd>/hello</kbd>  hello function will run and it will show us **hello** message on our web page . 


### Let start with writing code for it : 

First we will import 3 packages :-
1. fmt ( This package will be used for printing text and message on our web page .)
1. log  (This package will be used to log error if there any .)
1. net/http (This package will be used form http post and get requests .) 

```go
package main

import (
	"fmt"
	"log"
	"net/http"
)

```

Next step will be to create our main function and then creating a server using **FileServer()** function form our http package. <br>`fileServer := http.FileServer(http.Dir("./static"))`