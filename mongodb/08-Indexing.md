# Indexes

Indexes enable you you to performe quiries efficiently.
A database index is similar to a book's index. Instead of looking through the whole book, the database takes a shortcut and just looks an ordered list with references to the content.

a collection scan: is the query that does not use an index (the server has to look through the whole book, here the process is very slow for a large collection)

## Creating an index


    - db.users.createIndex({"username": 1})

try to query a collection of millions of documents

    - db.users.find({"username": "user10001"}).explain("executionStats)

Indexes have thier price: write operations (inserts, updates and deletes) that modify an indexed field will take longer

### Compoud Indexes (indexes with more than one key)

    - db.users.createIndex({"username": 1, "age": 1})
    - ................................................
    - db.users.find({"age": {"$gte": 21, "$lte": 30}})

    - db.users.find({"age": {"$gte": 21, "$lte": 30}}).sort({"username": 1}) // less efficient if dtata to sort is more than 32 MB Mongodb will return an error
