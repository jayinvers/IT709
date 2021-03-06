# Work on Server-side API with .Net 6

## Intro

This class teaches the basics of building a web API with ASP.NET Core. In this lesson we will use a Todo application as an example to show how to create a simple web API. Also we will create a simple React App to call this Todo web service.

## Objectives

- Create a web API project.
- Add a model class and a database context.
- Scaffold a controller with CRUD methods.
- Configure routing, URL paths, and return values.
- Use the web API with a React app.

## Overview

we are going to create the following API
![api of todo app](/pic/13_web_api_1/01.png)
![api of todo app](../pic/13_web_api_1/01.png)

The following diagram shows the design of the app.
![design of the todo app](/pic/13_web_api_1/02.png)
![design of the todo app](../pic/13_web_api_1/02.png)

## 1. Create a web project

- From the File menu, select New > Project.
- Enter Web API in the search box.
- Select the ASP.NET Core Web API template and select Next.
- In the Configure your new project dialog, name the project TodoApi and select Next.
- In the Additional information dialog:
  - Confirm the Framework is .NET 6.0 (Long-term support).
  - Confirm the checkbox for Use controllers(uncheck to use minimal APIs) is checked.
  - Select Create.

## 2. Test the project

- Press Ctrl+F5 to run without the debugger.

- The Swagger page /swagger/index.html is displayed. Select GET > Try it out > Execute. The page displays:

  - The Curl command to test the WeatherForecast API.
  - The URL to test the WeatherForecast API.
  - The response code, body, and headers.
  - A drop down list box with media types and the example value and schema.

## 3. add your first Model class

- A *model* is a set of classes that represent the data that the app manages. The model for this app is a single Todo class.

- In Solution Explorer, right-click the project. Select Add > New Folder. Name the folder Models.

- Right-click the Models folder and select Add > Class. Name the class TodoItem and select Add.

Replace the template code with the following:

```cs
namespace TodoApi.Models
{
    public class Todo
    {
        public int TodoId { get; set; }
        public string Title { get; set; }
        public bool IsDone { get; set; }
        public DateTime CreatedDate { get; set; }

    }
}
```

## 4. Scaffold a controller

- Right-click the Controllers folder.

- Select Add > New Scaffolded Item.

- Select API Controller with actions, using Entity Framework, and then select Add.

- In the Add API Controller with actions, using Entity Framework dialog:

  - Select TodoItem (TodoApi.Models) in the Model class.
  - Click + button and add TodoApiContext  in the Data context class.
  - Select Add.

  If the scaffolding operation fails, select Add to try scaffolding a second time.

### Explaination

The generated code:

- Marks the class with the [ApiController] attribute. This attribute indicates that the controller responds to web API requests. For information about specific behaviors that the attribute enables, see Create web APIs with ASP.NET Core.
- Uses DI to inject the database context (TodoApiContext) into the controller. The database context is used in each of the CRUD methods in the controller.
The ASP.NET Core templates for:

- Controllers with views include [action] in the route template.
- API controllers don't include [action] in the route template.

When the [action] token isn't in the route template, the action name is excluded from the route. That is, the action's associated method name isn't used in the matching route.

## 5. Update the PostTodo create method

Update the return statement in the PostTodo to use the nameof operator:

```cs
        // POST: api/Todoes
        [HttpPost]
        public async Task<ActionResult<Todo>> PostTodo(Todo todo)
        {
            _context.Todo.Add(todo);
            await _context.SaveChangesAsync();

            return CreatedAtAction(nameof(GetTodo), new { id = todo.TodoId }, todo);

        }
```

The preceding code is an HTTP POST method, as indicated by the [HttpPost] attribute. The method gets the value of the to-do item from the body of the HTTP request.

The CreatedAtAction method:

- Returns an HTTP 201 status code if successful. HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.
- Adds a Location header to the response. The Location header specifies the URI of the newly created to-do item. For more information, see 10.2.2 201 Created.
- References the GetTodoItem action to create the Location header's URI. The C# nameof keyword is used to avoid hard-coding the action name in the CreatedAtAction call.

## 6. Run and test the app

You can test the app on Swagger. (see the demo)


## 7. Create a new React app

Building a Todo App is easy and does not take much time but it teaches you some important concepts. It teaches you the principle of CRUD (Create, Read, Update and Delete) which are very important to understand for any developer.

Since this is our first project in the React world, we would keep things simple. We won’t be using Redux for state management and we would not use any kind of server to manage it.

Building a simple Todo list means we won’t be able to keep track of the todos once we refresh the page. So, it is not a perfect solution but a good start.


```bash
npm install create-react-app
create-react-app todo
cd todo
vscode .


```

Next, Open Terminal in VS Code and run:

```bash
npm start
```

## 8. Replace the code

Now we will show the basics of React in the form of a demo. (In class)

Here are some important things to know about React. js:

- https://reactjs.org/docs/hooks-state.html

Replacing the file App.js to following code:

```javascript
import React, { useState, useEffect  } from "react";
import {extend } from 'umi-request';
import './App.css';

const BASE_URL = 'https://localhost:7096';
const request = extend({
  prefix: BASE_URL,
  timeout: 1000,
});

function Todo({ todo, index, markTodo, removeTodo }) {
  return (
    <div className="todo">
      <span style={{ textDecoration: todo.isDone ? "line-through" : "" }}>{todo.todoId}. {todo.title}</span>
      <div>
        <button onClick={() => markTodo(index)}>✓</button>{' '}
        <button onClick={() => removeTodo(index)}>✕</button>
      </div>
    </div>
  );
}

function App() {

  const [loading, setLoading] = useState(false);
  const [todos, setTodos] = useState([]);
  const [todoText, setTodoText] = useState("");

  const addRandomTodorder= () =>{
    setLoading(true);
    request
    .post('/api/Todoes',{
      data:{
        Title: todoText,
        IsDone: false
      }
    }).then(res =>{
      addTodo(res);
      setTodoText("");
      setLoading(false);
    }).catch(error =>{
      setLoading(false);
    })
    
    

  };

  const addTodo = todo => {
    const newTodos = [...todos, todo];
    setTodos(newTodos);
  };

  const markTodo = index => {
    const newTodos = [...todos];
    newTodos[index].isDone = true;
    request.put( '/api/Todoes/'+todos[index].todoId,{data:todos[index]})
    setTodos(newTodos);
  };

  const removeTodo = index => {
    const newTodos = [...todos];
    request.delete( '/api/Todoes/'+todos[index].todoId)
    newTodos.splice(index, 1);
    
    setTodos(newTodos);
  };

  useEffect(() => {
    request
  .get( '/api/Todoes')
  .then(function(response) {
    setTodos(response)
  })
  .catch(function(error) {
    console.log(error);
  });
  },[]);

  return (
    <div className="App">
      <header>
        <h1>
        TODO List:
        </h1>
        <div>
        <input size="60" type="text" value={todoText} onChange={e => setTodoText(e.target.value)} />
        <button onClick={()=>addRandomTodorder()} disabled={loading}>Add</button>

        </div>
        
      </header>
      <section>
        <ul>
        {todos.map((todo, index) => (
          <Todo
          key={todo.todoId}
          index={index}
          todo={todo}
          markTodo={markTodo}
          removeTodo={removeTodo}
          />
        ))}
        </ul>        
      </section>
    </div>
  );
}

export default App;

```

## References

https://docs.microsoft.com/en-us/aspnet/core/tutorials/first-web-api?view=aspnetcore-6.0&tabs=visual-studio

https://towardsdatascience.com/build-a-simple-todo-app-using-react-a492adc9c8a4
