Practice Task- 2
 Mastering MongoDB Aggregation & Indexing
 1. Retrieve the count of individuals who are active (isActive: true) for each
 gender.
 2. Retrieve the names and email addresses of individuals who are active
 (`isActive: true`) and have a favorite fruit of "banana".
 3. Find the average age of individuals for each favorite fruit, then sort the
 results in descending order of average age.
 4. Retrieve a list of unique friend names for individuals who have at least
 one friend, and include only the friends with names starting with the
 letter "W".
 Hints: Explore how to use regex [ "friends.name": /^W/]
 5. Use $facet to separate individuals into two facets based on their age:
 those below 30 and those above 30. Then, within each facet, bucket the
 individuals into age ranges (e.g., 20-25, 26-30, etc.) and sort them by
 age within each bucket.
 6. Calculate the total balance of individuals for each company and display
 the company name along with the total balance. Limit the result to show
 only the top two companies with the highest total balance.
 Hints: Explore $slice, $split