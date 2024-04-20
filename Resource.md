# Manage Mockify Resources

![resources page](https://github.com/ARAldhafeeri/mockify-docs/blob/main/imgs/resourcespage.png?raw=true)

The resources page is where you manage mockify resources; it allows you to add, modify, and remove resources.  Two primary components define a resource:

1. Schema: Let users of Mockify specify the resource's schema by indicating the name and type of each field.
2. Features flags: enable users of mockify to adjust the level of resource security by having full control of enabling or disabling features such as the  ability to send put, delete, post, and get http requests.


Be aware that based on features that are enabled or disabled, generic endpoints will be constructed for each resource. The `<domainName>/mock/<resourceUID>} pattern will be followed by the generic endpoints. Under the URL column on the endpoint tab, you can see these generic endpoints.

## Create Resource

1. Click on the resources tab.
2. From the right-top corner at the end of the projects tab, click on `+`.
3. The create project modal will appear. Fill in the form.
4. Form steps:
   4.1. Basic info: Pick the project and resource name. Note: The resource name must be unique.
   4.2. Features: Enable and disable needed features for the project.
   4.3. Schema: Click on `+` in the schema step to add fields. Click on `-` to remove a field from the schema. For each field, you must add a name, type, and specify if the field is required or not.
   4.5. Policies: After finishing each step, you can move to the next step or go back to the previous step.
   4.6. Click on `create project`.

## Edit Resource

1. Click on the resources tab.
2. Click on the edit icon from the actions tab.
3. Edit the resource.
4. Click on `update resource`.

## Delete Resource

1. Click on the resources tab.
2. Click on the delete icon from the actions tab.
3. Click `yes` on the confirmation message.
4. The resource is deleted.

Note: Endpoints are generated based on resource features.
