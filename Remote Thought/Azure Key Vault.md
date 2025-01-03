
2025-01-03 11:44

Source: #Cloud
### Azure Key Vault

Azure Key Vault is an Azure service that allows users to securely store and access secrets. These secrets can be anything from API Keys, certificates, passwords, cryptographic keys, and more. 

The secrets are stored in vaults, which are created by vault owners. Vault owners have full access and control over the vault, including the ability to enable auditing so a record is kept of who accessed what secrets and grant permissions for other users to access the vault (known as **vault consumers**). 
### Microsoft Entra ID

Microsoft Entra ID (formerly known as Azure Active Directory)  is an identity and access management (IAM) service. In short, it has the information needed to assess whether a user/application can access X resource. 
### Azure Cloud Shell 

all the following commands are to be executed in the Azure Cloud Shell.

see authenticated user details.
```sh
az ad signed-in-user show
```
#### Entra ID Enumeration

**listing all the users in the tenant.**
```sh
az ad user list
```
Azure CLI typically uses the following command syntax:
`az GROUP SUBGROUP ACTION OPTIONAL_PARAMETERS`

**To filter Specific users** 
```
az ad user list --filter "startsWith('wvusr-', displayName)"
```

**listing the groups.**
```
az ad group list
```

**list the members of this group**
```
az ad group member list --group "Secret Recovery Group"
```

**Azure Role Assignments**

**Azure Role Assignments** define the resources that each user or group can access. When a new user is created via [[Entra ID]], it cannot access any resource by default due to a lack of role.
- To grant access, an administrator must assign a **role** to let users view or manage a specific resource.
- Additionally, **group members can inherit a role** when assigned to a group.

see if a role is assigned to the "Secret Recovery Group".
```sh
az role assignment list --assignee REPLACE_WITH_SECRET_RECOVERY_GROUP_ID --all
```

list the accessible key vaults
```sh
az keyvault list
```

list the secrets in the vault
```sh
az keyvault secret list --vault-name warevillesecrets
```

access the contents of the discovered secret with the **Key Vault Secrets User** role.
```
az keyvault secret show --vault-name warevillesecrets --name REDACTED
```
 Replace the value of the `--name` parameter with the actual secret name.

### References
Day 16

Azure roles [Microsoft documentation](https://learn.microsoft.com/en-us/azure/role-based-access-control/built-in-roles):