## Structure

![Api Structure](https://raw.githubusercontent.com/Nitwel/directus-wiki/main/api.svg)

| Name | Description |
| ---- | -------- |
| Rest | This is the Rest Api endpoint everyone can access. Handles incomming requests. |
| GraphQL | ---- |
| Services | Provide functions to create, edit or delete items inside the database. Turns every action into an AST object which then can be processed by the `run-ast` function. |
| run-ast | Interpets the AST object and with the use of knex, creates SQL Requests that go directly to the DB. |
| knex | Is a helper library that allows to easily create SQL statements, independend of which DB is used. |
| DB | Can be any Database like Postgres, Sqlite or MariaDB. |