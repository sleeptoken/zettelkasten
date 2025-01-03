
2025-01-03 11:44

Source: #Cloud

Tags: 

### Azure Key Vault

Azure Key Vault is an Azure service that allows users to securely store and access secrets. These secrets can be anything from API Keys, certificates, passwords, cryptographic keys, and more. Essentially, anything you want to keep safe, away from the eyes of others, and easily configure and restrict access to is what you want to store in an Azure Key Vault.

The secrets are stored in vaults, which are created by vault owners. Vault owners have full access and control over the vault, including the ability to enable auditing so a record is kept of who accessed what secrets and grant permissions for other users to access the vault (known as **vault consumers**). McSkidy uses this service to store secrets related to evidence and has been entrusted to store some of Wareville's town secrets here.
### Microsoft Entra ID

McSkidy also needed a way to grant users access to her system and be able to secure and organise their access easily. So, a Wareville town member could easily access or update their secret. Microsoft Entra ID (formerly known as Azure Active Directory) is Azure's solution. Entra ID is an identity and access management (IAM) service. In short, it has the information needed to assess whether a user/application can access X resource. In the case of the Wareville town members, they made an Entra ID account, and McSkidy assigned the appropriate permissions to this account.

With that covered, let's see what the Glitch has come up with.

all the following commands are to be executed in the Azure Cloud Shell.

see authenticated user details.
```sh
az ad signed-in-user show
```
#### Entra ID Enumeration

#### listing all the users in the tenant.
```sh
az ad user list
```
Azure CLI typically uses the following command syntax:
`az GROUP SUBGROUP ACTION OPTIONAL_PARAMETERS`

To filter Specific users 
```
az ad user list --filter "startsWith('wvusr-', displayName)"
```
####  listing the groups.

```
az ad group list
```

##### list the members of this group

```
az ad group member list --group "Secret Recovery Group"
```
### References
Day 16