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

In relational databases we want to normalize our data
