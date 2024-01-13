# Quick guide dynamic role-based access control

The policies page is for managing mockify policies, where you can create, edit, and delete policies. Policies can be used in edge functions to mock real-world APIs access control with ease thanks to the gatewatchJS library. The diagram below explains how the library works:

![gatewatch](https://github.com/ARAldhafeeri/mockify-docs/blob/main/imgs/gatewatch.png?raw=true)

For more information, visit the [gatewatchJS GitHub page](https://github.com/ARAldhafeeri/gatewatch).

---

## Manage Mockify Policies

### 1. Create Policy

1. Click on the policies tab.
2. From the top-right corner at the end of the policies tab, click on the "+" icon.
3. A modal will appear with form steps.
4. Form steps:
   4.1. Resource: Use the "+" icon to add resources to the policy and "-" icon to remove resources from the policy definition.
   4.2. Action: Use the "+" icon to add actions to the policy and "-" icon to remove actions from the policy definition.
   4.3. Roles: Use the "+" icon to add roles to the policy and "-" icon to remove roles from the policy definition.
   4.4. Role: Note that you can add as many roles, actions, and resources as the policy needs.
   4.5. After adding actions, resources, and roles, you can create policies as follows: `role roleName can perform [<actions>] on [<resources>]`.
   4.6. Click on the "Add Policy" button, type in the role name, and add the resources and actions added in steps 1-3.
   4.7. Preview the created policy/policies.
   4.8. Click on the "Create Policy" button.

### 2. Edit Policy

1. Click on the policies tab.
2. Click on the edit icon.
3. A modal will appear with form steps.
4. Form steps:
   4.1. Resource: Use the "+" icon to add resources to the policy and "-" icon to remove resources from the policy definition.
   4.2. Action: Use the "+" icon to add actions to the policy and "-" icon to remove actions from the policy definition.
   4.3. Roles: Use the "+" icon to add roles to the policy and "-" icon to remove roles from the policy definition.
   4.4. Role: Note that you can add as many roles, actions, and resources as the policy needs.
   4.5. After adding actions, resources, and roles, you can create policies as follows: `role roleName can perform [<actions>] on [<resources>]`.
   4.6. Click on the "Add Policy" button, type in the role name, and add the resources and actions added in steps 1-3.
   4.7. Preview the edited policy/policies.
   4.8. Click on the "Update Policy" button.

### 3. Delete Policy

3.1. Click on the policies tab.
3.2. Click on the delete icon in the actions column for the policy you want to delete.
3.3. Click "Yes".


## Example with edge functions and gatewatch 

Certainly! Here are examples of edge functions using Gatewatch, along with brief explanations for each:


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