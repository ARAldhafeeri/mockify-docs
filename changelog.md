# changelog

# 0.42.0

## Events

![events.drawio.png](https://raw.githubusercontent.com/ARAldhafeeri/mockify-docs/main/imgs/events.png?raw=true)

Events in [mockify.io](http://mockify.io) allow development  teams to mock event-driven architecture :

- Event name
- Event handler
- in-memory shared context Redis

**Control flow ( figure 1) :**

step-1) admin configure events in the admin portal

step-2 ) [Mockify.io](http://Mockify.io) uses NodeJS run time to create, update, delete events upon events REST API call 

step-3,step-4) events data are persisted to MongoDB

step-5) upon successful persistence the event is added to NodeJS EventEmitter. 

step-6) event is listening to an invoke from NodeJS runtime.

step-7) mockify client send request to an edge function with events in it.

step-8, step-9) edge function invoke events, NodeJS publish the event

step-10, step-11) event invoked with access to  shared memory ( redis db )

step-12,step-13, step-14 ) events invoked edge function return response 

shared memory recommendation : 

- For data needed for multiple events set them without TTL
- For data needed only to handle event, group of events, set them  with TTL to avoid over whelming the machine memory with events data.
- keys must be set in this fashion event:eventName

The Event then can be invoked within an edge function :

```jsx
await Emit('event:eventName', params)
```

The content of an event is business logic, therefore events have all edge function context, including Emit, to chain events.

The events page is divided into two main pages :

1. Manage: User interface for  creating, editing, deleting, viewing events.
2. Handlers: Business logic invoked when the events are fired.

# 0.41.0

## Redis cache and configuration

Cache page in [mockify.io](http://mockify.io) allow you to create, manage cache, get,set keys , view your cached data. There are three new endpoints  :

- Allow users mock cache and access it  within edge function
- Cache page lists k,v with create, delete, update abilities.
- POST  v1/cache/:projectName endpoint : Mock  API endpoint to set  k/v within cache
- GET v1/cache/:projectName  endpoint : Mock API endpoint to get  a k/v within cache
- DELETE v1/cache/:projectName  endpoint: Mock API endpoint to delete  a k/v within cache
- GET v1/cache/all /:projectName  return all cache data for a specific project.
- Multi-tenant cache database.

# multi tenancy in Redis

- Access to cache is  restricted per API key.
- APIKey is linked to projectName, projectName is there with every request url to the cache
- User set keys as plain strings e.g.

```json
SET SomeKey Value
```

Mockify implicitly hide the following detail from the user, ito achieve multi-tenancy , it will set the key with the following prefix  projectName:someKey

```json
SET ProjectName:SomeKey Value
```

Then getting all the keys  for the user is using the following wild card

```json
GET projectname:*
```

## Using cache within edge function

```json
CacheGet(k)
CacheSet(k,v)
```

The following functions are added to edge function context,  user can get, set keys using the added context. User can provide plain string,mockify will add projectName:key for each get, set request.

### Example covering mock cache requirement TTL

**Time-to-Live (TTL):** You can set an expiration time for each key in Redis using the TTL feature. After a specified period, the key will automatically be deleted. This is useful for automatically refreshing the cache periodically.

In [mockify.io](http://mockify.io) you can cover this requirement using newly added edge context 

```json
CacheGet
CacheSet
```

Example : 

- We defined function name EdgeTest
- For resource default

```json
await CacheSet('test:test', 'test', 1000);
data = await CacheGet('test:test');
```

Request :

```json
http://localhost:5000/v1/default/edge/edgeTest
```

Response:


# 0.40.0-beta 

## Mock endpoints

- bug fix: filter endpoint changed filtering “data?.filterName” to “data.filtername”

## Edge

- bug fix: api access  control

## Swagger

- Bug fixing in swagger generation :
    - base api url for test execution fixed
    - fix body of generated swagger api resources
    - fix try it out, added host name and correct paths.
    - fix body , query, params, headers
- Fix menu item for swagger page in navigation

## Endpoint

- added params
- added path
- added query
- headers

# 0.36.0-beta

### Documentation

- 0.33.0 typos in documentation, improvements, publishing .

### Swagger Page ( new )

- 0.34.0 swagger  button removed from endpoint page
- 0.35.0 bug fixing swagger docs generation
- 0.36.0 new  fully featured swagger ui for each created resource, generated and cached client side.

---
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
