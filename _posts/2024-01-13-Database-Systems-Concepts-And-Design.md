This study guide is a collection of my notes for the class CS 6400 (Database Systems Concepts And Design) at Georgia Tech.

## Chapter 1 - Databases and Database Users

### 1.1 | Introduction

The word *database* is so commonly used that we must first begin by defining what a database is.

In the simplest, most general terms, a **database** is a collection of data. By **data**, we mean known facts that can be recorded and have implicit meaning. 

For example, people's names, telephone numbers, addresses, etc..

However (for our needs) this definition of a database is *too general*; for example, we may consider the collection of words that make up this post to be related and hence to constitute a database. However, the common use of the term *database* is usually more restricted. A database has the following implicit properties:

- A database represents some aspect of the real world, sometimes called the **miniworld** or the **universe of discourse (UoD)**. Changes to the miniworld are reflected in the database
- A database is a logically coherent collection of data with some inherent meaning. A random assortment of data cannot be correctly referred to as a database
- A database is designed, built, and populated with data for a **specific purpose**. It has an intended group of users and some application(s) that interact with it. The users themselves interact with the application (user -> application -> database)

In other words, a database has some **source** from which data is derived, some degree of **interaction** with events in the real world, and an **audience** that is actively interested in its contents. In order for the database to be accurate and reliable at all times, it must be a true reflection of the **miniworld** that it represents. Therefore, changes must be reflected in the database as soon as possible.

A database may be generated and maintained manually or it may be computerized. A computerized database may be created and maintained either by a group of application programs written specifically for that task or by a database management system.

A **database management system (DBMS)** is a computerized system than enables users to create and maintain a database. The DBMS is a *general-purpose software system* that facilitates the processes of *defining, constructing, manipulating,* and *sharing* databases among users and applications. 

- **Defining** a database involves specifying the **data types, structures, and constraints** of the data to be stored in the database. The database definition is itself stored by the DBMS in the form of a database "catalog" or "dictionary"; it is callied **metadata**
- **Constructing** the database is the **process of storing the data** on some storage medium that is controlled by the DBMS
- **Manipulating** a database includes querying the database, updating the database to reflect changes in the miniworld, and generating reports from the data
- **Sharing** a database means allowing multiple users and programs to access the database concurrently

An **application** accesses the database by sending queries to the DBMS. A **query** causes some data to be retrieved; a **transaction** may cause some data to be read and some data to be written to the database.

Other important functions provided by the DBMS include protecting the database and maintaining it over time. **Protection** includes *system protection* against hardware or software malfunctions (crashes) or *security protections* against unauthorized access.

It is not absolutely necessary to use general-purpose DBMS software to implement
a computerized database. It is possible to write a customized set of programs to create and maintain the database, in effect creating a special-purpose DBMS software
for a specific application, such as airlines reservations.

Finally, to complete our initial definitions, we will call the database and DBMS software
together a **database system**. 

### 1.2 | Example

Consider an example, a university database to maintain information concerning:
- students
- courses
- sections
- grade report
- course prerequisites

The database is organized as five files, each of which stores data records of the same type.

First we need to **define** the database. For this, we must specify the structure of the records of each file by specifying the different types of **data elements** to be stored for each record as well as their **data types** (e.g., string, integer, decimal, etc..). 

To **construct** the university database we store data to represent each student, course, section, grade report, and prerequisite as a record in the appropriate file. Notice that records in the various files may be related. For example the record for student Smith in the STUDENT file is related to records in the GRADE_REPORT file that specify Smith's grades in two sections. Most databases include records that have many *relationships* between them.

Database *manipulation* involves querying and updating.

