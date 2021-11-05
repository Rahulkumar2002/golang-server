>## In this Project we will be creating a beginner friendly Server in Golang.  

To create it first we have to create our files and folder in our Code Editor , I'm using VSCode . VSCode is a Open Source code editor from Microsoft & it is quite awesome . 

We will create a **main.go** file and a **static folder** inside this **static folder** , we will create a file named as **index.html** and **form.html**. 

Inside index.html we will write some **HTML** code . Which will only show a text **Static Website** on our web page . Similary inside **form.html**  , we will create a form with two inputs , first input will be for name and second will be for address of the user . In our **main.go** we will write logic which will serve us file according to the end point hitted in the url . 

Let start building this project with our **index.html** file : 
    To get a basic html syntax like this just press <kbd>shift</kbd> + <kbd>1</kbd> a popup will show with an *Emmet Abbreviation* in your vscode then press enter. Simillar type of code will be given to you .

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
>## Index.html file
 In this we have simply created a static site which only shows two words **Static Website**. 

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
>## Form.html file
 In **form.html** we will create a form with  `method = "POST"` , so it can make post request with input data in it . 

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

>## Main.go File
Let move ahead and make our logic for this server which is our **main.go** file  . So what will be structure for this file . 


<!-- link for structure of image  -->
 ![Struture](https://raw.githubusercontent.com/Rahulkumar2002/golang-server/master/image/structImg.png)

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
>## Main Function
Next step will be to create our main function and then creating a server using **FileServer()** function form our http package. <br>`fileServer := http.FileServer(http.Dir("./static"))` . After this we will use **handler** function from http package and pass **fileServer** for <kbd>/</kbd> . We will use **handlerFunc** from http to assign handler function to different end points . 

```go
fileServer := http.FileServer(http.Dir("./static"))
	http.Handle("/", fileServer)
	http.HandleFunc("/form", formHandler)
	http.HandleFunc("/hello", helloHandler)

```
And to check for any error in connecting and handling function we will use **ListenAndServe** function from http package . 

```go
fmt.Println("Starting server port at :8080\n")
	if err := http.ListenAndServe(":8080", nil); err != nil {
		log.Fatal(err)

```

Our **main.go** function will look something like this :

```go
func main() {
	fileServer := http.FileServer(http.Dir("./static"))
	http.Handle("/", fileServer)
	http.HandleFunc("/form", formHandler)
	http.HandleFunc("/hello", helloHandler)

	fmt.Println("Starting server port at :8080\n")
	if err := http.ListenAndServe(":8080", nil); err != nil {
		log.Fatal(err)
	}

}

```
>## HelloHandler
Let jump right into creating our **helloHandler** function . Our **helloHandler** function will take two arguments first will be *http.ResponseWriter* & second will be *http.Request* `helloHandler(w http.ResponseWriter, r *http.Request) `. We will pust a check if some puts path as */hello* , then we will show him an error & also when someone try to use any other method except **GET** . Except these two condition , we will show **hello!** on our web page .

```go
func helloHandler(w http.ResponseWriter, r *http.Request) {
	if r.URL.Path != "/hello" {
		http.Error(w, "404 not found ", http.StatusNotFound)
		return
	}
	if r.Method != "GET" {
		http.Error(w, "method is not supported", http.StatusNotFound)
		return
	}
	fmt.Fprintf(w, "hello!")
}

```

>## FormHandler 

Moving to our last part **formHandler** function . Our **formHandler** function will take two argument similarly to our **helloHandler** . We will create a check for our request by using **ParseForm** function .
If there aren't any error we will print **POST request succesfull** & we will fetch input value from our form and assign it to variables and print it out on our webpage using **Fprintf** .



```go
func formHandler(w http.ResponseWriter, r *http.Request) {
	if err := r.ParseForm(); err != nil {
		fmt.Fprintf(w, "ParseForm() error %v", err)
		return
	}
	fmt.Fprintf(w, "POST request succesfull\n")
	name := r.FormValue("name")
	address := r.FormValue("address")
	fmt.Fprintf(w, "Name %s\n", name)
	fmt.Fprintf(w, "Address %s\n", address)
}

```

That's it . Our **main** function is complete , final code should look like this:

```go
package main

import (
	"fmt"
	"log"
	"net/http"
)

func formHandler(w http.ResponseWriter, r *http.Request) {
	if err := r.ParseForm(); err != nil {
		fmt.Fprintf(w, "ParseForm() error %v", err)
		return
	}
	fmt.Fprintf(w, "POST request succesfull\n")
	name := r.FormValue("name")
	address := r.FormValue("address")
	fmt.Fprintf(w, "Name %s\n", name)
	fmt.Fprintf(w, "Address %s\n", address)
}

func helloHandler(w http.ResponseWriter, r *http.Request) {
	if r.URL.Path != "/hello" {
		http.Error(w, "404 not found ", http.StatusNotFound)
		return
	}
	if r.Method != "GET" {
		http.Error(w, "method is not supported", http.StatusNotFound)
		return
	}
	fmt.Fprintf(w, "hello!")
}
func main() {
	fileServer := http.FileServer(http.Dir("./static"))
	http.Handle("/", fileServer)
	http.HandleFunc("/form", formHandler)
	http.HandleFunc("/hello", helloHandler)

	fmt.Println("Starting server port at :8080\n")
	if err := http.ListenAndServe(":8080", nil); err != nil {
		log.Fatal(err)
	}

}

```

After creating this we can run it . <br>

Step 1 : First run your main.go file in your code editor  , you it should show this message **Starting server port at :8080** .

Step 2 : Open your browser and type <kbd>localhost:8080</kbd>

Step 3 : When you hit <kbd>localhost:8080/</kbd> , server will serve you **index.html** file .

Step 4 : When you hit <kbd>localhost:8080/form.html</kbd> , server will serve you **form.html** file .

Step 5 : When you hit <kbd>localhost:8080/hello</kbd> **helloHandler** function will run and your webpage will show hello!. 

Thank u for reading this.

Feel free to fork this repo and make changes to the code base .
