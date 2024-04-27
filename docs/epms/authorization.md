---
sidebar_position: 5
---

# Authorization

## Role-Based Access Control (RBAC) Implementation

### Overview

This document provides an overview of the RBAC system implemented in the application. The system manages user roles and permissions dynamically, ensuring that only authorized users can perform certain actions based on their assigned roles.

### Database Schema

#### Roles

- **Model**: `Role`
- **Fields**:
  - `name`: String (unique, required)
  - `isActive`: Boolean (default: true, used for soft deletion)
- **Description**: Represents various roles in the system, such as Admin, User, Editor, etc.

#### Permissions

- **Model**: `Permission`
- **Fields**:
  - `role`: ObjectId (references `Role`, required)
  - `module`: String (required)
  - `read`: Boolean (default: false)
  - `write`: Boolean (default: false)
  - `update`: Boolean (default: false)
  - `delete`: Boolean (default: false)
  - `isActive`: Boolean (default: true, used for soft deletion)
- **Description**: Defines specific access controls for different modules (parts of the application), detailing what actions each role can perform.

### API Endpoints

#### Role Management

- **Create Role**
  - `POST /api/roles`
  - Body: `{ "name": "RoleName" }`
  - Description: Creates a new role.
- **Get All Roles**
  - `GET /api/roles`
  - Description: Retrieves all active roles.
- **Get Role by ID**
  - `GET /api/roles/:id`
  - Description: Retrieves a specific role by its ID.
- **Update Role**
  - `PUT /api/roles/:id`
  - Body: `{ "name": "NewRoleName" }`
  - Description: Updates the name of an existing role.
- **Soft Delete Role**
  - `PUT /api/roles/delete/:id`
  - Description: Deactivates a role (soft deletion).

#### Permission Management

- **Create Permission**
  - `POST /api/permissions`
  - Body: `{ "role": "RoleId", "module": "ModuleName", "read": true, "write": false, "update": true, "delete": false }`
  - Description: Assigns permissions to a role for a specific module.
- **Get All Permissions**
  - `GET /api/permissions`
  - Description: Retrieves all active permissions.
- **Get Permission by ID**
  - `GET /api/permissions/:id`
  - Description: Retrieves specific permission details by its ID.
- **Update Permission**
  - `PUT /api/permissions/:id`
  - Body: `{ "role": "RoleId", "module": "ModuleName", "read": true, "write": true, "update": true, "delete": true }`
  - Description: Updates an existing permission.
- **Soft Delete Permission**
  - `PUT /api/permissions/delete/:id`
  - Description: Deactivates a permission (soft deletion).

### Middleware for Authorization

- **checkPermission**
  - Arguments: `(module, action)`
  - Description: Middleware that checks if the user has the required permission for an action on a specific module.

### Best Practices

- Always ensure data integrity by preventing the deletion of roles and permissions that are currently in use.
- Use soft deletion for roles and permissions to maintain historical data and recover easily from accidental deletions.

### Conclusion

This RBAC system allows for flexible and secure management of user roles and permissions, ensuring that only authorized users can access specific functionalities within the application.
