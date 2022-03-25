# Foreground app with .NET 6.0 MVC architecture (part 4) Code Refactor

## 1. Intro

So far, we have built a well-working foreground website. It works, but it still leaves a lot to be desired. Some **duplicated code**, **non-reusable** pagination, some **hardcoded** content of the website. When we review the code, we need to dive into the **requirement** to **rethink** its functionality.

**Code refactoring** should **always** be on our minds. Writing reusable code not only increases productivity but makes code more readable and clear.

Today, we will dive into code refactoring. I'm going to take an example with **Pagination**, then talk about how to split controller and model to make the code reusable.

## 2. Objectives

- Use Fake data generator to fill the testing database
- Understanding fundamental of pagination
- Refactoring pagination code
- Creating a pagination NuGet package for other project

## 3. Requirement

Last time, we worked on the Contact page to allow visitors could submit a message to the database. The problem is that visitors leave too many messages, and when we look at the list of messages in the admin panel, we could get confused and could not tell which one is processed or not. Furthermore, loading hundreds of comments at once may cause the page to load slow, we need pagination.

We plan to design two tabs to identify messages before and after processing. All processed messages will go to the Processed tab. And let each page display a maximum of 4 items.

![prototype of messages](../pic/06_code_refactoring_pagination/01-prototype-message-admin-panel.png)

## 4. Fake data generatior

## 5. Coding with pagination

## 6. Code Refactor

## 7. Make your frist NuGet package

## 8. conclusion
