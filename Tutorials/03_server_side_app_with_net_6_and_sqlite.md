# Server-side App with .Net 6 and Sqlite

## 1. Intro

This doc teaches creating an ASP.NET Core MVC Web App with controllers and views. We will have an app that displays a personal CV.

**Objectives:**

- Create a web app with Visual Studio 2022
- Create Controller and View layers
- Using a model to work with databases (Sqlite or SqlServer)
- Upload files.

## 2. Prerequisites

- Visual Studio 2022
- Bootstrap
- SQLite Administrator

## 3. Create a new project

- Start Visual Studio and choose **Create a New Project**
- Select **ASP.NET Core Web App (Model-View-Controller)** and **Next**
- Project name: *MyWebSite*（name should be Camel-Case）and **Next**
- Framework: **.NET 6.0 (long-term support)** and check **Enable Docker**
- Create

![Additional information](../pic/03_server_side_app_with_net_6_and_sqlite/01-create-a-project.png)

- Change Run Mode from **Docker** to **MyWebSite** and **Run (F5)**

![Change Run Mode](../pic/03_server_side_app_with_net_6_and_sqlite/02-change-run-mode.png)

- See the result on the browser
  
![Welcome](../pic/03_server_side_app_with_net_6_and_sqlite/03-result.png)

## 4. New Page - add a controller

The Model-View-Controller (MVC) architectural pattern separates an app into three main components: Model, View, and Controller. The MVC pattern helps you create apps that are more testable and easier to update than traditional monolithic apps(Rick Anderson, 2022).

MVC-based apps contain:

- Models: Classes that represent the data of the app. The model classes use validation logic to enforce business rules for that data. Typically, model objects retrieve and store model state in a database. In this tutorial, a Movie model retrieves movie data from a database, provides it to the view or updates it. Updated data is written to a database.
- Views: Views are the components that display the app's user interface (UI). Generally, this UI displays the model data.
- Controllers: Classes that:
  - Handle browser requests.
  - Retrieve model data.
  - Call view templates that return a response.

In an MVC app, the view only displays information. The controller handles and responds to user input and interaction. For example, the controller handles URL segments and query-string values, and passes these values to the model. The model might use these values to query the database. For example:

**https://localhost:7177/Home/Privacy**: specifies the **Home** controller and the **Privacy** action.

![Controller and action](../pic/03_server_side_app_with_net_6_and_sqlite/04-controller-n-action.png)

### Add a controller

This section will build the controller for the **Contact** page.

- In Solution Explorer, right-click **Controllers** > **Add** > **Controller**.
  ![add a controller](../pic/03_server_side_app_with_net_6_and_sqlite/05-add-a-controller.png)
- Choose MVC Controller - Empty > Add.
  ![add new scaffolded item](../pic/03_server_side_app_with_net_6_and_sqlite/06-add-new-scaffolded-item.png)
- Name: ContactController.cs
  ![add new item](../pic/03_server_side_app_with_net_6_and_sqlite/07-add%20new%20item.png)

- Add following code after *Index()* method

```cs
        public string test()
        {
            return "test";
        }
```

- Run and Go to *https://localhost:7177/contact/test*
  ![contact-test](../pic/03_server_side_app_with_net_6_and_sqlite/08-contact-test.png)

- Customize Path for an action (*Contact/test*)
  
  We want to change *https://localhost:7177/contact/test* to *https://localhost:7177/contact-test*

  Copy following code replace test() method

  ```cs
        [Route("/contact-test")]
        public string test()
        {
            return "test";
        }
  ```

  ![Contact Controller](../pic/03_server_side_app_with_net_6_and_sqlite/09-contact-controller.png)

  **Run** and go to *<https://localhost:7177/contact-test>*

  ![result of customize an action](../pic/03_server_side_app_with_net_6_and_sqlite/10-result-customize-action.png)

## 5. Add a view

When we access *https://localhost:7177/contact*, will got an unhandled exception. Because we did not assign a view file(\*\.cshtml) to acthion *Index()*

![the unhandled exception](../pic/03_server_side_app_with_net_6_and_sqlite/11-an-unhandled-exception-without-view-file.png)

Now, let's start creating the new View file for the controller ContactController.

### Add a view file for action *Index()*

- Right-click **Views** > **Add** > **New Folder**

  Folder name: **Contact**
  
  ![create a folder for contact view](../pic/03_server_side_app_with_net_6_and_sqlite/12-create-a-folder-for-contact-view.png)

- Right-click **Contact** > **Add** > **View**
  
  ![add a view](../pic/03_server_side_app_with_net_6_and_sqlite/13-add-view.png)

- Replacing following code to **index.cshtml**

  ```cshtml
    @{
        ViewData["Title"] = "Contact Me";
    }

    <div class="text-center">
        <h1 class="display-4">Contact</h1>
        <p>If you have questions, feel free to send an email to me. maxazure@gmail.com</p>
    </div>
  ```

- Append following html to Views/Shared/_Layout.cshtml
  
  ```html
  <li class="nav-item">
     <a class="nav-link text-dark" asp-area="" asp-controller="Contact" asp-action="Index">Contact</a>
  </li>
  ```

  ![add contact to nav](../pic/03_server_side_app_with_net_6_and_sqlite/14-add-contact-to-nav.png)

### Self Work

Try to modify the **Privacy** page to **About**, and fill in some text as the content.

List of files that need to be modified:

- HomeController.cs
- _Layout.cshtml
- Privacy.cshtml

See the result:

![result of about page](../pic/03_server_side_app_with_net_6_and_sqlite/15-result-of-about.png)











### update EF-tools

```bash
dotnet tool update --global dotnet-ef
```
