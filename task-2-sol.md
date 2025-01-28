* Task-1
- Retrieve the count of individuals who are active (isActive: true) for each
 gender.
```
db.pracmd.aggregate([
    // matching
    {
        $match: {isActive: true}
    },
    // grouping and counting
    {
        $group: { _id: "$gender",
            activeCount: {$sum: 1}
        }
    }
])
```

* Task-2
- Retrieve the names and email addresses of individuals who are active
 (`isActive: true`) and have a favorite fruit of "banana".
```
db.pracmd.aggregate([
    // matching
    {
        $match: { isActive: true, favoriteFruit: "banana" }
    },
    // projecting email (optional)
    {
        $project: {email: 1}
    }
])
```

* Task-3
- Find the average age of individuals for each favorite fruit, then sort the
 results in descending order of average age.
```
db.pracmd.aggregate([
    // group by favoriteFruit
    {
        $group: {
            _id: "$favoriteFruit",
            avgAge: { $avg: "$age" }
        }
    },
    // projection 
    {
        $project: {
            avgAge: {
                $round: ["$avgAge", 2]
            },
            
        }
    },
    // sorting
    {
        $sort: { avgAge: -1 }
    }
])
```

* Task-4
- Retrieve a list of unique friend names for individuals who have at least
 one friend, and include only the friends with names starting with the
 letter "W".
  Hints: Explore how to use regex [ "friends.name": /^W/]
```
db.pracmd.aggregate([
    // unwinding doc for friends array 
    {
        $unwind: "$friends"
    },
    // matching friends name starts with "W"
    {
        $match: { "friends.name": { $regex: /^W/i } }
    },
    // grouping by individuals
    {
        $group: { _id: "$_id",
            uniqueFriends: {
                $addToSet: "$friends.name"
            }
        }
    }
])
```

* Task-5
- Use $facet to separate individuals into two facets based on their age:
 those below 30 and those above 30. Then, within each facet, bucket the
 individuals into age ranges (e.g., 20-25, 26-30, etc.) and sort them by
 age within each bucket.
```
db.pracmd.aggregate([
    {
        $facet: {
            // below 30
            "below-30": [
                {
                    $match: { age: { $lt: 30 } }
                },
                {
                    $bucket: {
                        groupBy: "$age",
                        boundaries: [20, 25, 30],
                        default: "Below 20",
                        output: {
                            names: { $push: "$name" }
                        }
                    },
                },
                {
                    $sort: { age: 1 }
                }
            ],
            // above 30
            "above-30": [
                {
                    $match: { age: { $lt: 30 } }
                },
                {
                    $bucket: {
                        groupBy: "$age",
                        boundaries: [30, 35, 40],
                        default: "Above 40",
                        output: {
                            names: { $push: "$name" }
                        }
                    },
                },
                {
                    $sort: { age: 1 }
                }
            ]
        }
    }
])
```

* Task-6
- Calculate the total balance of individuals for each company and display
 the company name along with the total balance. Limit the result to show
 only the top two companies with the highest total balance.
 Hints: Explore $slice, $split
```
db.pracmd.aggregate([
    // grouping by company
    {
        $group: {
            _id: "$company",
            totalBalance: {
                $sum:
                {
                    $convert: { input: { $substr: ["$balance", 1, -1] }, to: "double" }
                }
            }
        }
    },
    // sorting by highest total
    {
        $sort: { totalBalance: -1}
    },
    // limiting results
    {
        $limit: 2
    }
])
```
