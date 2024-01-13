# Edge Functions
Edge functions are functions that run code based on request method type and edge function name. They run code in a sandbox NodeJS VM with an enclosed context. The code running in those functions is defined in the admin portal of mockify.io, so they are trusted. In the future, approval, code review, and other improvements will be put in place to harden the security. Kindly read [Node.js VM docs](https://nodejs.org/api/vm.html).

## Edge Function URL Format
Once an edge function is defined, an endpoint is automatically generated in this URL format: `https://<domainName>/<resourceName>/edge/<edgeFunctionName>`.

## Context
Each edge function has access to the following context:

```javascript
{
  mongoose,
  ResourceModel,
  DataModel,
  ProjectModel,
  PolicyModel,
  EdgeModel,
  data: {},
  queries,
  headers,
  body,
  params,
  faker,
  safeRes : {
    status: null,
    message: null,
    headers: null
  }
};
```

### Context Description:
- `mongoose`: Mongoose instance within the sandbox
- `ResourceModel`: Mongoose model for resources
- `DataModel`: Mongoose model for data
- `ProjectModel`: Mongoose model for projects
- `PolicyModel`: Mongoose model for policies
- `EdgeModel`: Mongoose model for edge functions
- `data`: Data object for the response
- `queries`: Query params
- `headers`: Request headers
- `body`: Request body
- `params`: Request params
- `faker`: Faker instance within the sandbox
- `safeRes`: Safe response object, it has the following properties:
  - `status`: Response status code
  - `message`: Response message
  - `headers`: Response headers
  note  : if not set, the default values are:
  - `status`: 200
  - `message`: "fetch was successful"
  - `headers`: regular headers

Please review [Mongoose docs](https://mongoosejs.com/docs/models.html) for more information about mongoose models.

Please review [Faker docs](https://fakerjs.dev/) for more information about faker.

## Example:
```javascript
const { data, queries, headers, body, params } = context;
data = {
  "message": "Hello World"
};
```

The above code will run in a sandbox with the above context and return the following response:
```json
{
  "code": 200,
  "status": 200,
  "message": "fetch was successful",
  "data": {
    "message": "Hello World"
  }
}
```

---

# Manage Edge Functions

## Create Edge Function
1. Click on the edge tab.
2. Click on the create button.
3. Fill in the required fields.
4. Click on the create button.

## Update Edge Function
1. Click on the edge tab.
2. Click on the update button for the edge function you want to update.
3. Update the required fields.
4. Click on the update button.

## Delete Edge Function
To delete an edge function, click on the delete button in the edge function card you want to delete. A modal will pop up, click on the yes button.
```

---

# Examples with edge functions and faker


### Example 1: Delete a Resource Using Mongoose

```javascript

// Extract resource ID from request params
const resourceId = params.resourceId;

// Find the resource by ID and delete it
const deletedResource = await ResourceModel.findByIdAndDelete(resourceId);

// Update the response data
data = deletedResource;
```
This example shows how to delete a resource using Mongoose within the context of edge functions. Adjust the code according to your specific use case and data models. params, ResourceModel, and data are part of the context object. It's abstracted away from the code snippet for brevity.
### Example 2: Fetch Projects with Pagination Using Mongoose

```javascript

// Extract pagination parameters from query
const { page = 1, limit = 10 } = queries;

// Perform pagination query for projects
const projects = await ProjectModel.find({})
  .skip((page - 1) * limit)
  .limit(limit);

// Update the response data
data = projects;
```
This example showcase how to return projects with pagination using Mongoose within the context of edge functions. Adjust the code according to your specific use case and data models. queries, ProjectModel, and data are part of the context object. It's abstracted away from the code snippet for brevity.

### Example 3: Count Data Entries Using Mongoose

```javascript

// Count the total number of data entries in the database
const dataCount = await DataModel.countDocuments();

// Update the response data
data = dataCount;
```
This example showcase how to count data entries using Mongoose within the context of edge functions. Adjust the code according to your specific use case and data models. DataModel and data are part of the context object. It's abstracted away from the code snippet for brevity.

Certainly! Here are examples for edge functions using Faker and a brief explanation for each:

# Examples with edge functions and Faker

### Example 1: Generate Fake User Data

```javascript
// Generate fake user data using Faker
const fakeUser = {
  name: faker.name.firstName(),
  email: faker.internet.email(),
  username: faker.internet.userName(),
  // Add other fields as needed
};

// Update the response data
data = fakeUser;
```

This example demonstrates how to generate fake user data using Faker within an edge function. Adjust the fields and data structure according to your requirements.

### Example 2: Create Mock Product Entries

```javascript
// Generate multiple fake product entries for testing
const mockProducts = Array.from({ length: 5 }, () => ({
  name: faker.commerce.productName(),
  price: faker.commerce.price(),
  category: faker.commerce.department(),
  // Add other fields as needed
}));

// Update the response data
data = mockProducts;
```

This example shows how to create mock product entries using Faker within an edge function. Adjust the fields and the number of entries as per your testing needs.

### Example 3: Simulate Sensor Data

```javascript
// Simulate sensor data with random values
const sensorData = {
  temperature: faker.random.number({ min: -10, max: 40 }),
  humidity: faker.random.number({ min: 20, max: 80 }),
  status: faker.random.boolean(),
  // Add other fields as needed
};

// Update the response data
data = sensorData;
```

# Example use gatewatch and inforce policy on edge function 

```javascript 
const project = await ProjectModel.findOne({name: 'default'});
const policy =  await PolicyModel.findOne({project: project._id});
var ac = new AccessControl(policy);
var enforcedPolicy = ac.enforce();
const grant = new GrantQuery(policy).role('user').can(['getx']).on(['default']).grant();
if (grant){
  data = {
    "message": "Hello World"
  };
} else {
  safeRes.status = 403;
  safeRes.message = "forbidden";
}
// since there is a policy of role user can perform action of getx on resource default
// data will return true
// note : do not hard code policy query it from database
// if grant is false then safeRes will return 403 forbidden, data is {}
```

This example illustrates how to simulate sensor data using Faker within an edge function. Adjust the fields and the range of random values based on your specific scenario.

These examples provide a starting point for using Faker within edge functions, generating different types of fake data for various purposes.
