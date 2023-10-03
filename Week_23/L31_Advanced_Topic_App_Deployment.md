# Lecture 31: Advanced Topic - Deploying Apps & CI/CD

### What is CI/CD?
* Continuous Integration / Continuous Delivery (or Continuous Deployment)
* CI - integrate the pieces of code / branches into main / master
  * Handling multiple developers submitting code in perhaps different styles, using different IDEs or operating systems, etc.
  * Testing the new code to ensure it doesn't break anything and works as intended
* CD - deploy the app with the new pieces of code integrated
  * Use of *web hooks* to update the changes in the database, call the API, etc.
* This larger view of the entire development process encompasses *DevOps*, a specialist developer role

### Web Dev Review
* UX: Figma, Keynote Powerpoint
* Frontend: HTML, CSS, JavaScript, jQuery, Reactjs
* Backend: Nodejs/Express, Ruby/Rails, PHP/Laravel, Python/Django/Flask, Java/Spring, C#/.NET
* Database: SQL, noSQL, MySQL, PostgreSQL

### Getting a Project to Run on a New Computer
- NodeJs/Git installed
- Clone repository
- Install project dependencies
- Setup database
  Can set-up in three ways:
    1. Database Driver, like psql. Speaks the same language as the database, but risky as its injecting right into the code. i.e., ```db.query(INSERT INTO...)```
    2. Query Builder 
    ```js
      db(products)
        .insert(newProduct)
        .returning(*)
    ```
    3. ORM (Object Relation Mapping)
- Set the environment variables (.env, .env.development)
- Running scripts

### Connecting a database with ElephantSQL
* [https://www.elephantsql.com/](https://www.elephantsql.com/)

### Deploying Nodejs using Render
* [https://render.com/](https://render.com/)

### Deploying Reactjs using Netlify
* [https://www.netlify.com/](https://www.netlify.com/)