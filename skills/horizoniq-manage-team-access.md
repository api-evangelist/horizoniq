---
name: Manage HorizonIQ Compass team access
description: Invite users, assign and revoke roles, and toggle admin permissions for a HorizonIQ Compass account.
api: openapi/horizoniq-compass-openapi-original.yml
operations: [getUsers, getUsersRoles, createUsers, updateUsersByUserUUIDRoles, updateUsersByUserUUIDRolesByRoleUUID, updateUsersByUserUUIDAdminEnabled, updateUsersByUserUUIDAdminDisabled, deleteUsersByUserUUID]
---

# Manage HorizonIQ Compass team access

Administer user accounts and role-based access on the HorizonIQ Compass API (`https://api.compass.horizoniq.com/v1`).

## Auth
`Authorization: Bearer <token>` from a Compass account with administrator permissions. HTTPS required.

## Steps
1. **Review the team.** `getUsers` (GET `/users`) lists user accounts; `getUsersRoles` (GET `/users/roles`) returns the available roles to assign.
2. **Add a user.** `createUsers` (POST `/users`) with the new user's details.
3. **Grant a role.** `updateUsersByUserUUIDRoles` (PATCH `/users/{userUUID}/roles`) adds a role; `updateUsersByUserUUIDRolesByRoleUUID` (PATCH `/users/{userUUID}/roles/{roleUUID}`) removes one.
4. **Toggle admin.** `updateUsersByUserUUIDAdminEnabled` / `updateUsersByUserUUIDAdminDisabled` (PATCH) grant or remove administrator permissions.
5. **Remove a user.** `deleteUsersByUserUUID` (DELETE `/users/{userUUID}`) deletes the account.

## Rules
- Only administrators can manage users; a non-admin token yields a 400/unauthorized response.
- Resolve the `userUUID` via `getUsers` before any PATCH/DELETE — never guess it.
- Errors are HTTP status + JSON `message` (not RFC 9457). See errors/horizoniq-problem-types.yml.
