# Hasura

<img src="Hasura GraphQL.assets/Screen Shot 2022-03-31 at 12.29.40 PM.png" alt="Screen Shot 2022-03-31 at 12.29.40 PM" style="zoom:50%;" />

<img src="Hasura GraphQL.assets/Screen Shot 2022-03-31 at 5.07.00 PM.png" alt="Screen Shot 2022-03-31 at 5.07.00 PM" style="zoom:50%;" />

Hasura provide a way to translate any graphql query to SQL to access the database, which is good for an serverless app like Next.js

<img src="Hasura GraphQL.assets/Screen Shot 2022-03-31 at 5.09.21 PM.png" alt="Screen Shot 2022-03-31 at 5.09.21 PM" style="zoom:50%;" />

## Create Database with Heroku

<img src="Hasura GraphQL.assets/Screen Shot 2022-03-31 at 9.33.52 PM.png" alt="Screen Shot 2022-03-31 at 9.33.52 PM" style="zoom:50%;" />

<img src="Hasura GraphQL.assets/Screen Shot 2022-03-31 at 9.40.24 PM.png" alt="Screen Shot 2022-03-31 at 9.40.24 PM" style="zoom:50%;" />

Now the GraphiQL is ready to go:

<img src="Hasura GraphQL.assets/Screen Shot 2022-03-31 at 9.41.05 PM.png" alt="Screen Shot 2022-03-31 at 9.41.05 PM" style="zoom:50%;" />

We can also see and manipulate the same datadase table in SQL way:

<img src="Hasura GraphQL.assets/Screen Shot 2022-03-31 at 9.45.11 PM.png" alt="Screen Shot 2022-03-31 at 9.45.11 PM" style="zoom:50%;" />

Set permission for user:

Allow only the user him/herself(with issuer set in header as "X-Hasura-User-Id") have permission to change the row with the same issuer

<img src="Hasura GraphQL.assets/Screen Shot 2022-04-01 at 12.13.21 AM.png" alt="Screen Shot 2022-04-01 at 12.13.21 AM" style="zoom:50%;" />