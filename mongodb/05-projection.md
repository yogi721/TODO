# Projection

projection means selecting only the necessary data.

example: if I want to select only the title and exclude the id field witch is always returned by default

    - db.mycol.find({i_id: 0, "title": "third title"})