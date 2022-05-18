# .Net 6 Advanced - Designing and Implementing an Admin Panel

## Intro

In a web project, it is usually the admin panel that takes up the most development time. It usually contains some complex database query tasks, such as reports, product details modification. So, be sure to consider the database structure before designing it. Also, how to organize the project structure becomes important as we are faced with multiple levels of directory structure such as forground, admin panel and API.
So, today we're going to discuss how to design a working backend for your Project 2.

## Objectives

- Understand how to organize an admin panel directory structure with routing.
- Complex data query with Entity Model
- Display and modify complex data models

## Project structure

Although we want the directory structure of the project to be as simple as possible. But in real projects, we usually face some complex requirements that lead to more and more bloated directory structures.
Next, take a look at our common needs:

### Use directories for functional division (Area)

![admin panel structure](/pic/11_admin_panel/01.png)

This example is a common requirement, it requires the admin panel and API under the same domain name as the website (in the same .Net project)

**advantage:**

1. Simple and easy to learn
2. The structure is clear, and it is very convenient to modify the model

**Disadvantage:**

1. **Not suitable** for large-scale projects, especially when there are many team developers.
2. The business logic layer is highly coupled with the presentation layer (almost all business logic is written in the controller).

[An example link: StoreYx](https://github.com/jayinvers/StoreYx)

### Use projects for functional division

![Use projects for functional division](/pic/11_admin_panel/02.png)

This example is a popular project organization method in the industry. It divides different modules into different projects. When the project is built, different modules will be compiled into dll files respectively. The main project will call them through dll files.

Obviously, it will speed up the compilation time of the main project in large projects, since the solution does not need to be recompiled in its entirety.

**Please note**: When you organize your solution this way, you need two domains to deploy your project. Or proxy different ports through Nginx and map to different directories.

**Advantage:**

1. Highly reusable
2. Suitable for multiple developers
3. Low coupling
4. Fast compilation speed

**Disadvantage:**

1. Complicated, not suitable for beginners
2. Not suitable for simple projects

## Complex data query with Entity Model

Database access has always been a main task in regular .NET projects. And the complex queries it uses are mainly concentrated in admin panel projects. Common tasks such as reports, charts, related queries of multiple tables, joint queries, etc.

In the last lesson, we learned Complex Model Design. In this tutorial, we will give examples based on the specific business logic of each project team.

### Definition of terms

There are a number of terms used to describe relationships

- **Dependent entity**: This is the entity that contains the foreign key properties. Sometimes referred to as the 'child' of the relationship.

- **Principal entity**: This is the entity that contains the primary/alternate key properties. Sometimes referred to as the 'parent' of the relationship.

- **Principal key**: The properties that uniquely identify the principal entity. This may be the primary key or an alternate key.

- **Foreign key**: The properties in the dependent entity that are used to store the principal key values for the related entity.

- **Navigation property**: A property defined on the principal and/or dependent entity that references the related entity.

  - **Collection navigation property**: A navigation property that contains references to many related entities.

  - **Reference navigation property**: A navigation property that holds a reference to a single related entity.

  - **Inverse navigation property**: When discussing a particular navigation property, this term refers to the navigation property on the other end of the relationship.

- **Self-referencing relationship**: A relationship in which the dependent and the principal entity types are the same.

The following code shows a one-to-many relationship between **Blog** and **Post**

```cs
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

//  "= new List<Post>()" for Avoid null values when creating objects
    public List<Post> Posts { get; set; } = new List<Post>();
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public int BlogId { get; set; }
    public Blog Blog { get; set; }
}
```

**Post** is the dependent entity

**Blog** is the principal entity

**Blog.BlogId** is the principal key (in this case it is a primary key rather than an alternate key)

**Post.BlogId** is the foreign key

**Post.Blog** is a reference navigation property

**Blog.Posts** is a collection navigation property

**Post.Blog** is the inverse navigation property of **Blog.Posts** (and vice versa)

#### The above code will generate SQL code (SQLite)

```sql
CREATE TABLE "Blog" (
	"BlogId"	INTEGER NOT NULL,
	"Url"	TEXT NOT NULL,
	CONSTRAINT "PK_Blog" PRIMARY KEY("BlogId" AUTOINCREMENT)
);

CREATE TABLE "Posts" (
	"PostId"	INTEGER NOT NULL,
	"Title"	TEXT NOT NULL,
	"Content"	TEXT NOT NULL,
	"BlogId"	INTEGER NOT NULL,
	CONSTRAINT "FK_Posts_Blog_BlogId" FOREIGN KEY("BlogId") REFERENCES "Blog"("BlogId") ON DELETE CASCADE,
	CONSTRAINT "PK_Posts" PRIMARY KEY("PostId" AUTOINCREMENT)
);

```

**NOTE**: **ON DELETE CASCADE** constraint is used in MySQL to delete the rows from the child table automatically, when the rows from the parent table are deleted.

ERD:

![erd of blog](/pic/11_admin_panel/03.png)

### Create a record with relationship(one to many)

The following is a sample code that creates a Blog data and inserts two Post records in association.

<https://github.com/jayinvers/MyProject2/blob/master/MyProject2/Models/SeedData.cs>

```cs

Blog blog = new Blog();
blog.Url = "http://www.goo.com";

Post post1 = new Post
{
    Title = "hello",
    Content = "A test content.",

};
Post post2 = new Post
{
    Title = "hello2",
    Content = "A test content 2.",

};

// If declared unassigned.
//
// blog.Posts = new List<Post>();

blog.Posts.Add(post1);
blog.Posts.Add(post2);

context.Blogs.Add(blog);

context.SaveChanges();

```

### Access the data with relationship(one to many)

Use the Include keyword to perform a joint query, the code is as follows:

```cs
var blog = await _context.Blogs.Include(blog => blog.Posts)
                .FirstOrDefaultAsync(m => m.BlogId == id);
```

Multiple includes can be used if there are multiple subtables.

```cs
var blog = await _context.Blogs.Include(blog => blog.Posts)
                .Include(blog => blog.Tags)
                .FirstOrDefaultAsync(m => m.BlogId == id);
```

High-performance query methods can be used in large-scale queries.

```cs
var blog = context.Blogs.Where(d => d.Id == 1).Take(1);

blog.Include(p => p.Posts).SelectMany(d => d.Posts).Load();
blog.Include(t => t.Tags).SelectMany(d => d.Tags).Load();
blog.Include(c => c.Categories).SelectMany(d => d.Categories).Load();
```

In View
<https://github.com/jayinvers/MyProject2/blob/master/MyProject2/Areas/Admin/Views/Blogs/Details.cshtml>

```html
@model MyProject2.Models.Blog

@{
    ViewData["Title"] = "Details";
}

<h1>Details</h1>

<div>
    <h4>Blog</h4>
    <hr />
    <dl class="row">
        <dt class = "col-sm-2">
            @Html.DisplayNameFor(model => model.Url)
        </dt>
        <dd class = "col-sm-10">
            @Html.DisplayFor(model => model.Url)
        </dd>

    </dl>
    <ul>
        @foreach (var item in Model.Posts)
        {
            <li>@item.Title</li>
        }
    </ul>


</div>
<div>
    <a asp-action="Edit" asp-route-id="@Model?.BlogId">Edit</a> |
    <a asp-action="Index">Back to List</a>
</div>

```

## Consider the real situation in our project

Example 1

![Example 1](/pic/11_admin_panel/03.jpg)

Problems:

- Entity name should be singular (Orders -> Order)
- Field case should be in Pascal notation (Order_ID -> OrderId)

Example 2

![Example 2](/pic/11_admin_panel/05.png)

Example 3

![Example 3](/pic/11_admin_panel/06.png)

- Relationships need to be marked in the diagram
- Perhaps missing the **cart** and **store** tables.

## Tasks in class

- Build project (including identity)
- Create admin panel (by using Area)
- Create Entity Models (one to many)
- Add data management (eg: product management) under /admin


## references

<https://docs.microsoft.com/en-us/ef/core/modeling/relationships>
