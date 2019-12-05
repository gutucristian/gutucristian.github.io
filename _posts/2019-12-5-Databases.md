Keys in databases allow us to identify a record (a.k.a., tuple).

There are a few types of keys in databases:
1. super key
2. candidate key
3. primary key

Super keys: super key stands for superset of a key. A super key is a set of one or more attributes that are taken collectively and can identify all other attributes uniquely.

For example if we have a table `Book` with the domain `(BookId, BookName, Author)` then the following could all be super keys:
- `(BookId)`
- `(BookId, BookName)`
- `(BookId, Author)`
- `(Author, BookName)`
- `(BookId, BookName, Author)`

Each of these super keys is able to uniquely identify each record in our table.

Candidate keys are super keys which do not have any redundant attributes. Thus, they are a subset of all super keys. For example, from our set of super keys only the following are candidate keys:
- `(BookId)`
- `(Author, BookName)`

A redundant attribute in a key is an attribute which is not needed to uniquely identify a record. In the key `(BookId, BookName)` the attributed `(BookName)` is redundant because we can identify a book using just `(BookId)`.

A primary key is a candiate key that is chosen by the DB designer to identify entities (a.k.a., records or tuples) within an entity set. Once we select a candidate key from the cadidate keys set, the rest are called alternate keys.

A prime attribute is an attribute that is a member of some candidate key.

A nonprime attribute is not a prime attribute and is not a member of any candidate key.

In relational databases we want to normalize our data. The degree of normalization depends in what normal form out data is.

First normal form: 1NF
A relational schem R is in first normal form if all of its underlying domains contain atomic values only.

Second Normal Form: 2NF         
A relation schema is in second normal form if every non-prime attribute A in R is not partially dependent on a key. For example, consider the functional dependencies:

FDs = {AB -> C, B -> C}

This is not in 2NF because C is only partially dependent on the key AB. It is only partially dependent on AB because we can still identify C only knowing B which is only part of the key.

Consider another example. Say we have a table SCORE with the domain `(score_id, student_id, subject_id, score, teacher)`. In this table we can set our primary key to be `(student_id, subject_id)` (this is also known as a composite primary key). Evidently, we can use this primary key to uniquely identify a score for a student in some subject. However, the attribute `teacher` can be uniquely identified only using `subject_id`. Thus, `teacher` is partially dependent on our key which is made up of the attributes `(student_id, subject_id)`. Hence, this is not following 2NF.

Third Normal Form: 3NF
A relation schema is in third normal form if it is in 2NF and 1NF and every non-prime attribute A in R is non-transitively dependent on key. To be in 3NF whenever a FD X -> A holds in R, either (a) X is a superkey of R or (b) A is a prime attribute of R.

BCNF:
A relation is in Boyce-Codd Normal Form if for every X->A in R X is a superkey of R.
