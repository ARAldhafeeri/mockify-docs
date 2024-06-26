# Manage Mockify Users

![gatewatch](https://github.com/ARAldhafeeri/mockify-docs/blob/main/imgs/userspage.png?raw=true)

The users page is for managing Mockify admin portal users, and mock users for mock API endpoints and projects. There are 3 default types of users:

1. Super Admin
2. Admin
3. User

Super Admin is the main admin of the platform and can perform all actions, currently there is only one per deployment for security reasons. Admins can do everything except managing users. Users can only manage their own projects. To manage users, you need to be a Super Admin.

Currently, users can view projects created by them. In the near future, we will enable administrators to add users to specific projects so they can collaborate on a single project.


For example, an admin can create projects, resources, and handover endpoints to developers, but they are not necessarily required to know how to code and write edge functions or manage mock data, caches, and websockets.

## Create Super Admin

Super admin is created by default via project configuration. You can create other Super Admins using the Super Admin credentials:

1. Click on the Users tab.
2. Click on Create User.
3. Fill in the form.
4. Click on Create User with the role Super Admin.

## Edit User

1. Click on the Users tab.
2. Click on Edit User.
3. Fill in the form.
4. Click on Update User.

## Delete User

1. Click on the Users tab.
2. Click on Delete User.
3. Click yes on the confirmation message.
4. User is deleted.

Note: You can't delete, create, edit the Super Admin.
