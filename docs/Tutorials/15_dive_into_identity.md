# Dive into Identity - User Reg & Login with mutiple Roles

Today, let's take a deeper look at Microsoft identity. This is a great opportunity to learn about the features we can achieve with the help of Microsoft Identity.
We will implement user authorization related functions through an example of an online bookstore. This tutorial will show how to use **roles** to separate admin users from consumer users.

## Objectives

- Learn how to create your own login, logout and register functions without an identity template
- Learn how to use UserManager, and RoleManager to create users or roles.
- Learn how to create an order with a user ID.

## Lab Task

- Create a registration page on your own by following this tutorial.

## Project Source Code

https://github.com/jayinvers/MyBookShop

An example of online book store based on asp.net 6.

This source code has only a list of books that you can download as a starting point for our Lab.

![preview of mybookshop](../pic/15_dive_into_identity/1.png)


## TODO

- Add a login page.
- Display UserName on the topmenu of page(_Layout)
- Specify the login jump path according to the user's role. (Example: Admin goes to /admin, and Consumer goes to /)
- Implement the buy book action with UserId.

## Step 1. Clone the base project code local

```bash
git clone https://github.com/jayinvers/MyBookShop.git
cd MybookShop
```

Open the project file **MyBookShop.sln**

## Step 2. Install Identity in PMC

## Step 3. Configure Indetity in **Program.cs**

## 
