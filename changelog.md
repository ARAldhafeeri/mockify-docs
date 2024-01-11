# changelog

# 0.27.0-beta

### All pages

- 0.13.0 bug fix: any page with a tabs filter now loads the first entity upon page load.

### Projects

- 0.14.0 super admins have access to all projects; users, admins, and users have access only to projects created by them.
- 0.15.0 project user drop-down menu

### Resources

- 0.18.0 added: object, array, and ref types added to schema creation to create schema for more advanced mock data requirements. Avoided ref as we can use number as int id in SQL databases, string as ref for NoSQL databases.

### Data

- 0.21.0 added: object, array, and ref types added to FormMaker to manage mock data from the admin portal

### Endpoint

- 0.22.0 feature: generate swagger 2.0 for each resource and cache them client-side.

### Edge function

- 0.24.0 added: FakerJS library to allow returning fake data instead of reading or writing from a database.

### User

- 0.25.0 Role < admin, user > dropdown menu in users page

### Docs

- 0.27.0: Renovate documents by using MD files and GitHub repos; add more examples.

---

# 0.7.0-beta

### Projects

- user can create, delete, update, edit projects

### Resources

- user can create, delete, update, edit resources

### Data

- user can create, delete, update, edit data

### Endpoint

- user can create, delete, update, edit endpoint

### Edge function

- user can create, delete, update, edit edge functions

### User

- admins can create, delete, update, edit users

### Policy

- user can create, delete, update, edit policies