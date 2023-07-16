# Breakout Session: LightBnB Q&A

### Creating our Schema

* Since we are using the CASCASE keyword, technically we would only need to include the first line, as users connect through all of the other tables.

01_schema.sql
```sql
DROP TABLE IF EXISTS users CASCADE;
-- DROP TABLE IF EXISTS properties CASCADE;
-- DROP TABLE IF EXISTS reservations CASCADE;
-- DROP TABLE IF EXISTS property_reviews CASCADE;
```

* If we weren't using the keyword CASCADE, we would need to drop them in the opposite order we created them (i.e., drop from most dependent to least dependent)

```sql
DROP TABLE IF EXISTS property_reviews;
DROP TABLE IF EXISTS reservations;
DROP TABLE IF EXISTS properties;
DROP TABLE IF EXISTS users;
```

* In psql, if you use the \d (instead of the \dt like we have been using), we will get all of our tables PLUS our "sequence" tables
  * If you need to change the ordering of the serial ID numbers (i.e., start from a higher number), you would do it by accessing these tables

### JOIN Options
Remember the three options you have for JOINS:
  1. JOIN (which is INNER JOIN by default): will only show results that have relationship between the two tables
  2. LEFT JOIN: will show everything from the left table and only results that have relationship from the right table
  3. RIGHT JOIN: will show everything from the right table and only results that have relationship from the left table

* If you do not include a LEFT JOIN with this query, you will not be able to see any new properties, as it will only show properties that have reviews.
03_properties_by_city.sql
```sql
SELECT properties.id, title, cost_per_night, 
  AVG(rating) AS average_rating
FROM properties
LEFT JOIN property_reviews ON property_id = properties.id
...
```

### SQL Execution Order
1. FROM's & JOIN's
2. WHERE
3. GROUP BY
4. HAVING
5. SELECT
6. DISTINCT
7. ORDER BY
8. LIMIT / OFFSET

* This is why you can't access an alias in SELECT in a HAVING clause, as the HAVING runs before the SELECT.

### Reading through the Code Base
* A great place to start: server.js!
  * This is the "brains" of the operation that connects to everything else (both the frontend and the database)

server.js
* /routes &rarr; rather than having all the routes in one big file, in bigger apps you will often find them organized in seperate files as mini "routers" in a directory
  * Each router can be thought of a mini Express app, only responsible for the routes in its particular file
  * Often you find that you have one router per table

database.js
* You can require json files, and they will be available to you and ready to use! No need to convert them to JSON before accessing.
```js
const properties = require("./json/properties.json");
```

### Final Tips
* While not required, practice storing sensitive data in a seperate env file
* If you see the error like "cannot read property of .then of undefined", that means you forgot to return the whole promise query!