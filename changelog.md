# changelog

# 0.32.0-beta

### Edge function

- 0.28.0 bug fix, add gatewatch to sandbox context AccessControl, GrantQuery is Added.
- 0.29.0 feat : res, allows you to set response headers , status, message, backward compatible:
    - res = {headers: [], status : 200, message: “success” , statusBoolean: false}

not setting res , default response :

```json
{"status":true,"message":"fetching data was successful","data": {} } 200 OK
```

 Setting  res as follow in the sand box : 

```JavaScript
safeRes.httpStatus = 201;
      safeRes.message = 'custom message';
      safeRes.headers = {
        'x-custom-header': 'custom header value'
       }
      sefRes.status = true;
      data = { test : 'test' }
```

yields the following response : 

```json
			{"status":false,"message":"custom header value","data": {"test" : "test"} } 201 BAD_REQUEST with header x-custom-header
```

Important note : if you want to use gatewatch and enforce policy the policy must be saved in PolicyModel and read, otherwise statically typing the policy cases a bug in node vm, somehow nested json error out within the sandbox , example 

```JavaScript
const project = await ProjectModel.findOne({name: 'default'});
    const policy =  await PolicyModel.findOne({project: project._id});
    var ac = new AccessControl(policy);
    var enforcedPolicy = ac.enforce();
    const grant = new GrantQuery(policy).role('user').can(['getx']).on(['default']).grant();
    data = grant
// since there is a policy of role user can perform action of getx on resource default
// data will return true
// note : do not hard code policy query it from database
```

### All pages

- 0.30.0  enhancement  & bug fixing
    - decrease width of submit button for web responsiveness.
    - enhanced appearance and responsiveness of modals.
    - enhanced appearance and responsiveness of form steps

### Policy

- 0.32.0 fix table show resources, actions, roles in dedicated columns
  
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
