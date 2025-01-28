* Task-1
- Find all documents in the collection where the age is greater than 30, and
 only return the name and email fields.
```
db.pracd.find(
    { age: { $gt: 30 } },
    { _id: 0, name: 1, email: 1 }
)
```

* Task-2
- Find documents where the favorite color is either "Maroon" or "Blue."
```
db.pracd.find(
    { favoutiteColor: { $in: ["Maroon", "Blue"] } }
)
```

* Task-3
- Find all documents where the skill is an empty array.
```
db.pracd.find(
    { skills: { $size: 0 } }
)
```

* Task-4
- Find documents where the person has skills in both "JavaScript" and
 "Java."
 ```
db.pracd.find(
    { "skills.name": { $all: ["JAVA", "JAVASCRIPT"] } }
)
 ```

* Task-5
- Add a new skill to the skills array for the document with the email
 "amccurry3@cnet.com". The skill is
 {"name": "Python", "level": "Beginner", "isLearning": true}
 Note: At first, you will have to insert the given email then add the skill
 mentioned above
 ```
db.pracd.updateOne(
    { email: "amccurry3@cnet.com" },
    {
        $addToSet:
        {
            skills: { "name": "Python", "level": "Beginner", "isLearning": true }
        }
    }
)
 ```

* Task-6
- Add a new language "Spanish" to the list of languages spoken by the
 person.
```
db.pracd.updateOne(
    { email: "amccurry3@cnet.com" },
    {
        $push: { languages: "Spanish" }
    }
)
```

* Task-7
- Remove the skill with the name "Kotlin" from the skills array.
```
db.pracd.updateOne(
    { email: "amccurry3@cnet.com" },
    {
        $pull: {"skills": {name: "KOTLIN"}}
    }
)
```
