---
layout: single
title: "[Java] TIL Java Spring Boot - Movie Review Site 001"
categories: Java
tag: [Java, Spring Boot, TIL]
toc: true
published: true
---

![](https://velog.velcdn.com/images/devbang/post/44b25767-0d3c-4c6a-92fc-6542a8abf306/image.png)

# Full Stack Development with Java Spring Boot, React, and MongoDB – Full Course

### This is the course from freeCodeCamp to learn Java with react

> You can find the video on YouTube here:
> https://youtu.be/5PdEmeopJVQ?si=uyg8MHc6LDpfd3kw

# 🧩 Problem

## Environment setup

Since this is the first time using JDK, I will briefly review the environment setup. I won't go too deep about the core Java in this post.

### JDK install

To install JDK, we need to go to the Oracle webpage or use the following command if you are a Mac user.

```
$ brew install openjdk@17
```

![](https://velog.velcdn.com/images/devbang/post/fb4c098f-865a-4482-b166-3afef8d3b193/image.png)

I used **homebrew** to install it to avoid any error related to the m1 chip.

After installing, run the following codes to check if JDK and JRE(Java Runtime Environment) are successfully installed on your local machine.

```
$ javac --version

$ java --version
```

![](https://velog.velcdn.com/images/devbang/post/f1d1a0b4-d2e3-48d8-83a3-22845c91f713/image.png)

### IntelliJ IDEA

Next, we need a code editor or IDE for **Java**.

We choose IntelliJ IDEA in this course, so go to the website and choose the correct version. They had an open-source version, so I installed the open-source one.

![](https://velog.velcdn.com/images/devbang/post/71e60cbd-a81c-42d0-bba3-88dab1d10948/image.png)

# 🎯 Database Setup

### MongoDB Setup

It is not a MongoDB course, so I skipped details about making a cluster, but if you follow the tutorial, it is pretty simple.

We will use this password to connect MongoDB with our Java backend, so leave a memo or copy this password for later.

![](https://velog.velcdn.com/images/devbang/post/e7bd840c-4147-449f-97b5-fe588afa853f/image.png)

### IP address setup

In this course, the instructor uses `0.0.0.0/0` to access `everywhere` for educational purposes.

But for security purposes, once the company or individual sets up the database, the database manager has to set it up with only allowed IP addresses. (e.g.) home(private), employees(company), etc...)

![](https://velog.velcdn.com/images/devbang/post/25b956e1-cad6-4675-8244-5c5fa68474c6/image.png)

### MongoDB Compass setup

In this course, we are using MongoDB Compass to interact with GUI.

![](https://velog.velcdn.com/images/devbang/post/051f225b-6add-4158-838f-0be2cae7a841/image.png)

You must install the correct OS version if you have yet to install Compass.

![](https://velog.velcdn.com/images/devbang/post/c90b73ad-e4e9-4b9d-a72a-2b0307afbff1/image.png)

The important thing is the second part. Copy the string from section two and paste it into the URI area.

Fix the `<password>` part with **_your password_** that we copied previously.

> The example is from the YouTube video, which already pasted passwords.

![](https://velog.velcdn.com/images/devbang/post/db38f0f2-9d9c-478e-85d1-a68e7e18cb65/image.png)

### Creating Database

We don't have to touch admin and local to create a new database. Click the plus button next to the database, and type the db name and collection name.

![](https://velog.velcdn.com/images/devbang/post/90b05db8-ad56-4947-b74b-880e8b952855/image.png)

Since MongoDB is a non-relational database(NoSQL) and document-based database, we first have the database name and then the collection name to manage data. Collections are treated as tables in relational databases like MySQL or Oracle.

In this project, we will have the **collection** of movies, where we save different information about the movies.

![](https://velog.velcdn.com/images/devbang/post/0d020388-91f6-46a2-930e-287eb3f395da/image.png)

### Import JSON file to the collection

It is time to import the dataset from somewhere. In this course, we will use a prepared JSON file. If you download and import the JSON file on the Compass, the result will be the same as below.

![](https://velog.velcdn.com/images/devbang/post/b064ef13-d1fa-4689-ad94-7b0974b14f2a/image.png)

# 📌 Takeaway

This series could be longer than expected due to the screenshots and the course content. I will reduce the content and screenshots because I'm studying this, not teaching.

# 💻 Solution

- None

# 🔖 Review

- In the real world, the database is not open to anybody. In MongoDB, setting up and allowing different IP addresses is used to solve this security problem.

- MongoDB is NoSQL(stands for Not only SQL), known as a non-relational database.
  The most significant difference compared to relational databases such as Oracle or MySQL is that MongoDB does not have tables.
  Instead, they save the data in documents. Each document has a collection to manage the data. In this project, we have a `movie-api-db > movies` structure.
