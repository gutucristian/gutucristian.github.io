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

The design of a new application for an existing database or design of a
brand new database starts off with a phase called **requirements specification and
analysis**. 

These requirements are documented in detail and transformed into a
**conceptual design** that can be represented and manipulated using some computerized tools so that it can be easily maintained, modified, and transformed into a
database implementation (Entity-Relationship model). 

The design is then translated to a **logical design** that can be expressed in a data model implemented in a commercial DBMS.

The final stage is **physical design**, during which further specifications are provided for storing and accessing the database. The database design is implemented, populated with actual data, and continuously maintained to reflect the state of the miniworld.

### 1.3 | Characteristics of the Database Approach

A number of characteristics separate the database approach from the much older approach of writing custom programs to access data stored in files.

In traditional **file processing** each user defines and implements the files needed for a specific software application as part of programming the application. For example, one user, the grade reporting office, may keep files on students and their grades. Custom features like printing a student's transcript and to enter new grades are implemented as part of that application. A second user, the accounting office, may keep track of student's fees and their payments. Although both users are interested in data about students, each maintains their own separate files -- because each requires some data not available from the other user's files. This workflow results in **redundancy** and **wasted storage space** on top of making it **more complicated to maintain common data**.

In the database approach, the source of data is in a **single source of truth**. In other words, a single repository maintains our data. This data is defined once and can be accessed by various users repeatedly through queries, transactions, and programs.

The main characteristics of the database approach vs the file-processing approach are the following:
- Self-describing nature of a database system
- Isolation between programs and data, and data abstraction
- Support of multiple views of the data
- Sharing of data and multi-user transaction processing

### 1.3.1 | Self-Describing Nature of a Database System

A fundamental characteristic of the database approach is that **the database system not only contains the database itself but also a complete description of the database structure and constraints** (structure of each file, type and format of each data item, etc). This description is stored in the **DBMS catalog** and is called the **meta-data** -- which (again) is describing the structure of the primary database.

It is important to note that some newer types of database systems, known as NOSQL systems, do not require meta-data. Rather the data is stored as **self-describing data** that includes the data item names and data values together in one structure (e.g., JSON). 

The catalog is used by the DBMS software and by users who need info on the database structure. 

In traditional file processing, data definition is part of the program itself. Therefore programs are tightly coupled with that definition. This is not the case with using a database which provides an abstraction over reading the actual data and we simply interact with the database. It handles all the details of how to read the data based on its structure and format using the description it has stored in the data catalog.

--------------------------------------------------------------

## Lecture Notes

Conceptual schema -> refers to meaning of data
- Describes **structural** aspects of reality. For example, the table name, column names

External schema -> refers to use of data
- External schema here basically refers to a database "view". It is derived from the "conceptual schema" in the sense that we use information like the underlying table name and column names to define the view definition (i.e., the query to create the view)
- Note a view is not an actual table, it is created in memory at run time 

Internal schema -> refers to storage of data
- Describes how the info in the conceptual schema is **physically** stored to provide the best performance (e.g., defining an index, or multiple, using a datastructure like a B+ tree which allows logarithmic time access to data which helps run queries faster)

These three schema levels in the ANSI-SPARC architecture allows for two types of independence

1. Physical data independence: a measure of how much the internal schema can change without affecting the application programs. Basically, we can add more indexes, insert/remove rows and still our application is completely decoupled and does not care or need to know about the internal logic and rules of how this data is read off the disk -- this is also known as separation of concerns. There is a similar concept in object oriented programming called encapsulation. As long as the method signatures remain the same, the implementation can change and the consumers will not be affected.

2. Logical data independence: a measure of how much you can change the conceptual schema without changing the application that run on top of the external schema. It is more difficult to provide logical data independence than logical data independence because the external schema is directly linked to the definition of the conceptual schema. If we change the underlying table definition the view (which uses that) is also affected and needs to be changed

### Entity Relationship Model (ER) and Extended Entity Relationship Model (EER)

#### Constraints

Keys

Primary keys:
- An attribute that uniquely identifies a row in a table
- No other entry in the database can have the same value for this attribute
- Primary keys CANNOT be NULL
- When an attribute that is a primary key in one table is used in another table, the following must be obeyed: the list of all the attributes used in the other table must be a **subset** of the values of the primary key attribute in the main table

Entity integrity

Referential integrity
