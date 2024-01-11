# Manage Mockify Resources

The resources page is for managing mockify resources, where you can create, edit, and delete resources. Note that for every resource, generic endpoints will be created based on enabled/disabled features. The generic endpoints will follow this pattern: `<domainName>/mock/<resourceName>`. You can view these generic endpoints in the endpoint tab under the URL column.

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
