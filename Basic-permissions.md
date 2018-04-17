## Users and roles

By using a hierarchy of roles, the administrator can create groups of users and permissions, or connect them directly.

The following diagram shows the Rhetos entities in the *Common* module that determine the user's roles and permissions. The diamond shapes represent the associative entities.

[[images/claims.png|Roles and Claims]]

### Common administration activities

All administration activities are performed by modifying the data in the entities from the diagram above. For example:

* To create a new user, insert the record in the *Common.Principal* entity.
* To configure the user's permissions, enter the data in *Common.PrincipalHasRole* or *Common.PrincipalPermission*.
* The roles and the role permissions are usually preconfigured by the development team (entering the data in *Common.Role* and *Common.RolePermission*).
* If the forms authentication is used, instead of the Windows authentication, check for the additional administration activities in the [AspNetFormsAuth documentation](https://github.com/Rhetos/AspNetFormsAuth/blob/master/Readme.md).

### Additional features

* For **automatic synchronization** of Rhetos user roles with the user groups in **Active Directory**,
add the [ActiveDirectorySync](https://github.com/Rhetos/ActiveDirectorySync) plugin package to the Rhetos application. This will allow the domain administrator to indirectly set the user permissions in the Rhetos application.
* The permission check can be turned off in a development environment by setting the following values in the Rhetos server's *web.config* file, inside the *appSettings* element:
    * "BuiltinAdminOverride" - If set to "True", the user that is a local administrator will have full permissions. This option will not work if the web server is not run in elevated mode (on IIS Express, e.g.); use the following option in that situation.
    * "Security.AllClaimsForUsers" - The value contains a comma-separated list of users (formatted `username@servername`) that automatically have full permissions.
* If "AuthorizationAddUnregisteredPrincipals" is set to "True" (in *web.config* file), an entry will automatically be inserted to the *Common.Principal* table for each new user on the first login. This user will have no permissions by default, but the additional functionality can be developed to automatically add the initial roles or permissions.

## Claims

Each record in a *Common.Claim* entity represents a basic element of access. is an operation on an entity (read/insert/update/delete) or an action.

The list of claims is created on deployment, and does not change during run-time.
There are claims generated for each of the following objects:

* For each entity: Read, New, Edit, Remove
* For each read-only data structure: Read
* For each action: Execute
* For each report: Download

A custom claim can be created in a DSL script by using the CustomClaim concept. Example:

    CustomClaim 'SomeModule.SomeResource' 'CustomOperationName';

## See also

* [User authentication and authorization](https://github.com/Rhetos/Rhetos/wiki/User-authentication-and-authorization) - Other options for user authorization.