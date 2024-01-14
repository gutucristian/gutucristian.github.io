This study guide is a collection of my notes for the class CS 6400 (Database Systems Concepts And Design) at Georgia Tech.

## Chapter 1 - Databases and Database Users

### 1.1 | Introduction

The word *database* is so commonly used that we must first begin by defining what a database is.

In the simplest, most general terms, a **database** is a collection of data. By **data*, we mean known facts that can be recorded and have implicit meaning. 

For example, people's names, telephone numbers, addresses, etc..

However (for our needs) this definition of a database is *too general*; for example, we may consider the collection of words that make up this post to be related and hence to constitute a database. However, the common use of the term *database* is usually more restricted. A database has the following implicit properties:

- A database represents some aspect of the real world, sometimes called the **miniworld** or the **universe of discourse (UoD)**. Changes to the miniworld are reflected in the database
- A database is a logically coherent collection of data with some inherent meaning. A random assortment of data cannot be correctly referred to as a database
- A database is designed, build, and populated with data for a **specific purpose**. It has an intended group of users and some application(s) that interact with it. The users themselved interact with the application (user -> application -> database)
