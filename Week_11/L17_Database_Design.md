# Lecture 17: Database Design

### Royalty in our Projects
What is the most important/core piece of any web project? &rarr; Typically, the DATA!

### Naming Conventions
* snake_case
* Naming the primary key either just id or specific to the table (i.e. id or post_id), naming the foreign key specific to the table its referencing (i.e. author_id)
```
blog
  posts
    id (PK)
    author_id (FK)
    title
    content

   authors
    id (PK)
    name
    bio
    age
```
* Note: this represents a *one-to-many* relationship, where one author can have many posts

### PostgreSQL Data Types
* See the [documentation](https://www.postgresql.org/docs/current/datatype.html) for the full list
  * Useful to have this list open when you are creating your database, as it's easier to pick the correct data type up-front than trying to update the data later!

Some common data types:
* INTEGER - due to its simplicity and universality, this can be used for all kinds of creative things, not just plain numbers. Examples include prices (in cents) and dates (in epoch milliseconds).
* TEXT - plain string of unlimited length
* BOOLEAN - true / false, just like usual

These common data types are great because if you ever decide to transfer to a different DBMS, these types are fairly universal and there will be less work to do on your end.

Some data types to generally avoid:
* ENUM - assigned values of options that you can store
  * Usually recommended to avoid this if you have lots of custom types and opt for multiple booleans instead
* ARRAY - create an array
  * Opt for reorganizing the data in another way (i.e. creating other tables) rather than have this nested grouping of information
* COMPOSITE - grabbing data from one or more other properties and "double-storing" it in a different format
  * Opt instead to use JS to put data together in the frontend, as there is potential for errors when storing it this way
  * Example: full_name COMPOSITE that fails to update when you update the first_name

### Relationships
1. **One-to-One** - relatively uncommon. Often if you find you are using a one-to-one relationship, you can find a way of amalgamating the two tables together.
  * You do find this used as a space-saving technique, i.e. if only some users have bios (which includes multiple fields), you don't need to store all the empty properties for all those users

***

2. **One-to-Many** - i.e. one author with many posts
```
blog
  posts
    PK id
    FK author_id

  authors
    PK id
```

***

3. **Many-to-Many** - i.e. many authors with many posts (i.e. can have multiple authors on the same post)
```
blog
  posts
    PK id
  
  authors
    PK id

  // pivot / bridge table
  post_authors
    PK id
    FK post_id
    FK author_id
```
* The pivot / bridge table can be used multiple times to store multiple authors
  * So, the mere presence of a pivot table can be a clear indicator that there is a many-to-many relationship happening!

### Design Concepts
**Required Fields** - mandatory fields that cannot be empty; if they are, the entry will not be entered
  * In HTML forms, can use the required keyword on the inputs that are mandatory; the submit button will not submit if these are empty
  * In databases, you use the keyword NOT NULL

***

**Default Values** - a fall-back value if no value was specified, some examples:
  * on_sale | DEFAULT=false
  * active | DEFAULT=true

***

**"Soft Delete" / "Safe Delete"** - not immediately / permanently deleting but otherwise transforming the data so it seems deleted to the users, some examples:
  * set active to false to no longer show some content / features to a user
  * posts deleted fron posts table move to deleted_posts table, which perhaps permanently deletes from deleted_posts after some specified period of time

***

**DRY!**
  * Example: instead of writing out your category for each post, have a seperate categories table and instead reference the relevant foreign key. This not only reduces the possibility for errors (like simple spelling errors) but can also allow for easy updating if any category name needs to change.

### ERD - Entity Relationship Diagram
A consistent way / format to visually represent a database, its tables, and their columns.
* Not focused on displaying any particular records, but rather, communicating its overall schema / structure

Use [this tool](https://app.diagrams.net/) to start creating your own ERD! This should be your first step in creating your database.
* Specifically label each property as either NOT NULL or NULLABLE