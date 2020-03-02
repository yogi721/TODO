# Indexes
Indexes enable you you to performe quiries efficiently.
A database index is similar to a book's index. Instead of looking through the whole book, the database takes a shortcut and just looks an ordered list with references to the content.

a collection scan: is the query that does not use an index (the server has to look through the whole book, here the process is very slow for a large collection)

## Creating an index
    - db.users.createIndex({"username": 1})

try to query a collection of millions of documents

    - db.users.find({"username": "user10001"}).explain("executionStats)