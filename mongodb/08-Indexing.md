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


    - // less efficient if dtata to sort is more than 32 MB 
    - // Mongodb will return an error
    - db.users.find({"age": {"$gte": 21, "$lte": 30}}).sort({"username": 1}) 

### Using Compound Indexes
    Compound indexes are a little more complicated, but very powerful

    Here we will walk trough an example that gives you an idea of the type of thinking you need to do when you are designing compound indexes

    The goal is for our read and write operations to be as efficient as possible.

    There are some best practices:
        1. we need to consider the selectivity of the index.
        2. we need to consider selectivity of all operations necessary to satisfy a query
        3. we need to consider how sorts are handled.
    For the example we will use a student dataset containing approximately 0ne million records:

    - {
        "_id": ObjectId("79d8f7....."),
        "student_id": 0,
        "scores": [
            {
                "type": "exam",
                "score": 38.0500321344 
            },
            {
                "type": "quiz",
                "score": 79.0501524344 
            },
            {
                "type": "homework",
                "score": 74.1501726543
            },
            {
                "type": "homework",
                "score": 74.6501726543 
            }
        ], 
        "class_id": 127
    }

    We will begin with two indexes and look at how MongoDB uses these indexes(or doesn't) in order to satisfy queries.

    - db.students.createIndex({"class_id": 1})
    - db.students.createIndex({student_id: 1, class_id: 1})

    Consider the following query, which illustrates several of the issues that we have to think about in designing our indexes

    - db.students.find({student_id:{$gt: 500000}, class_id: 54})
                .sort({student_id: 1})
                .explain("executionStats")

    Here we are requesting all records with an ID greater than 500,000 (about half of the records).
    There are about 500 classes represented in this dataset.
    Finally, we are sorting in ascending order based on "student_id" (the field on which we are doing a multivalue query)

    If we run the query, the output og the "explain" method tells us how MongoDB uses indexes to satisfy it:
        "totalKeysExamined" => this is how many keys MongoDB walked through
        "executionTimeMillis" => the time this query took in milliseconds


    The cursor "hint" methode enables us to specify a particular index to use

        - db.students.find({student_id:{$gt: 500000}, class_id: 54})
                .sort({student_id: 1})
                .hint({class_id})
                .explain("executionStats")

    The resulting output shows that we are now down from having scanned roughly 850,000 index keys to just about 20,000.
    In addition, the execution time is only about 270 millisecondes rather than 4.3 seconds

    Recap: when designing a compound index:
        keys for equality filters should appear first.
        keys used for sorting should appear before multivalue fields.
        keys for multivalue filters should appear last.

        - db.students.createIndex({class_id: 1, final_grade: 1,  student_id: 1})

        Query:

        - db.students.find({student_id:{$gt: 500000}, class_id: 54})
                .sort({final_grade: 1})
                .explain("executionStats")

##### Choosing key directions

##### using covered queries

##### implicit indexes

### How $ operators Use indexes