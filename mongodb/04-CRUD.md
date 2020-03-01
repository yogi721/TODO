# Create (insert)
### insert() or save()
#### db.collectionName.insert(document)
    - db.mycol.insert({
        _id: ObjectId(7df78ad8902c),
        title: ('MongoDB insert ),
        description: 'MongoDB is NoSQL database',
        by: 'Yogi721',
        url: 'https://linkedin.com/mehdi-semlali-1980',
        tags: ['mongodb','database', 'NoSQL', 'insert'],
        likes: 1000
    })

    if the collection doesn't exest, Mongodb will create it, then insert the document into it

#### db.collectionName.insert([{multi},{documents}])
    - db.mycol.insert([
        {
        _id: ObjectId(7df78ad8902c),
        title: ('MongoDB insert ),
        description: 'MongoDB is NoSQL database',
        by: 'Yogi721',
        url: 'https://linkedin.com/mehdi-semlali-1980',
        tags: ['mongodb','database', 'NoSQL', 'insert'],
        likes: 1000
        },
        {
        _id: ObjectId(52f78ad89123),
        title: ('Second doc ),
        description: 'MongoDB two',
        by: 'Yogi721',
        url: 'https://linkedin.com/mehdi-semlali-1980',
        tags: ['mongodb','Multiple', 'documents', 'insert'],
        likes: 1000
        }
    ])


# Read (Query)
### find() 
    - db.mycol.find().pretty()
#### and (equivalent to 'where' in RDBMS)
    - db.mycol.find(
        {
            $and:[
                {"by": "yogi721"},
                {"title": "Second doc"}
            ]
        }
    )

###### in mysql (where by = 'yogi721' AND title = 'Second doc')

#### or (RDBMS => where by = 'yogi721' AND title = 'Second doc')
    - db.mycol.find(
        {
            $or:[
                {"by": "yogi721"},
                {"title": "Other title"}
            ]
        }
    )

#### AND and OR together
    - db.mycol.find(
        {
            "likes": {$gt:10}
            $or:[
                {"by": "yogi721"},
                {"title": "Other title"}
            ]
        }
    )

# Update
### update() or save()
###### udate()
    - db.mycol.update(
        {"title": "Other title"},
        $set: {"title": "Other title 2"}
    )

###### save()
    - db.mycol.save({
        _id: ObjectId(7df78ad8902c),
        "title": "third title",
        "by": "newUser"
    })

# Delete
### remove
###### db.colname.remove(critteria)
    - db.mycol.remove({ "title": "third title" })
    - .............................................
    - db.mycol.remove({ ord_qty: { $gt: 100} })