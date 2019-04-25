# TASK-simple-todo-app-with-Go
In this case I created a directory todoapp then i created the main.go file inside it. The next step was i created a nother directory called it static then i created a home.tpl file inside it which will handel the frontend of the application.

We are using chi-router for routing, mgo for mongodb to persist our data and renderer to render response. You can install those packages using go get see the command below:

$ go get github.com/go-chi/chi
$ go get gopkg.in/mgo.v2
$ go get github.com/thedevsaddam/renderer

-- main.go file description
1. The first line is going to hold data type of pointer to renderer.Render and second line pointer to mgo.Database. See the code between line 27–33, where we initialized those two variables.
2. between line 4–9 we declared a constant block to define some constants like host name, database name, port etc.
3. Between line 11–15 we declared two struct todo and todoModel. They both have the same number of fields, fields name also remain same. But in todoModel the ID field is a type of bson.ObjectId. We used bson tag so that mgo package can use the tags. And in our todo we used json tag so that json decoder can use these tags to serialize the Go type to typical json.

4. Between line 54–57 we wrote a function homeHandler which is going to render our home page using the render package.

5. Between line 190-221 we wrote our main function which is our entry point. Inside main function we created a channel type of os.Signal and we register the channel to os.Signal package with an argument os.Interrupt which basically listen to os signals, if use signal cancel (ctrl+c) then notify the channel about the signal. We are using this signal to shutdown our server gracefully. Then we are declaring and initializing a variable as chi router. Then we are using a middleware to called Logger which comes with chi router as a sub-package, it basically log our incoming requests to stdout. Then we declared a http.Server server variable and initialized it using struct literal. Then we started a goroutine and fired up our server. Then we are releasing the signal from the blocked channel and we are declaring a context with time out of 5 sec and passing it to the server shout down method. Then we are using defer statement as soon as the main function ends the deferred cancel is called and release the context resources. And we mounted a route group called todo and we registered a route group which contains 4 routes. 

We wrote our createTodo function which is responsible for creating a new todo. we declared a variable of type todo and we decode the incoming request json data to our t variable, if it fails we return a json reply to user using renderer.JSON() method. Then we checked if the title is empty then we send a json reposne to user. Then we declared tm and initialized using struct literal with the incoming data form t variable and other information. Then we inserted the data to mondo db. Finally we sending a json reply to user that the todo created with the todo_id

Thats it!
