# Azure CLI Command Reference
> Azure Administrator (AZ-104) — Command reference for all topic areas

Azure CLI is a tool that lets you control Azure by typing commands. It works on Windows, Mac, and Linux, and uses a syntax style borrowed from Linux to create an ease of use, which then uses an Application Programming Interface (API) to send those commands to Azure. Azure CLI is particularly useful for developers and engineers who are already familiar with Linux systems as it matches their preference and background.

---

## How to Read This Document

Azure CLI follows a consistent three-part syntax structure. First is the tool, always `az`. Second is the target, the group or area within Azure you want to work with. Third is the action, what you want to do with it.

| Part | What it means | Example |
|------|---------------|---------|
| `az` | The tool — always first | `az` |
| Target | What you are targeting | `group`, `vm`, `storage account` |
| Action | What you want to do with it | `create`, `list`, `delete`, `show` |

**Full example:** `az group create --name "rg-demo" --location "uksouth"`

---

## Parameter Syntax

The parameter syntax is designed to keep consistency with the CLI, making it easier to identify mistakes and implement commands correctly. Flags preceded by double dashes are present to let Azure know the where and what, followed by the value surrounded by quotations.

| Syntax Rule | Example |
|-------------|---------|
| Flags use a double dash | `--resource-group`, `--location`, `--name` |
| Value follows with a space | `--location "uksouth"` |
| Strings with spaces need quotes | `--name "my resource group"` |
| Strings without spaces do not | `--location uksouth` |

---

## Getting Started

The first command to be written in the Azure Command Line Interface (CLI) would be the login of the service to ensure access is granted to the relevant area. This is similar to the web portal but rather than clicking an empty box to fill in your credentials, you are telling Azure the command to carry out the task. Once logged in you would select your subscription and whether you are going into development or production, similar to first and second fix, where first fix is the staging area, installing the cabling and accessories, before production in second fix ensuring the testing is completed before going live.

| Action | Command |
|--------|---------|
| Log in to Azure | `az login` |
| List all subscriptions | `az account list --output table` |
| Show current active subscription | `az account show` |
| Set active subscription | `az account set --subscription "your-subscription-id"` |

---

## 1. Identity & Governance

Identity and Governance is all about who people are, what their roles are, and the areas they can access. Similar to an office where an ID card is the identification but also has a chip for location access, the commands in Azure are similar to the security checks of the ID card when entering a building or area. The main purpose of these commands is to assign roles and responsibilities to individuals or groups, ensuring that someone cannot gain access to a service they are not permitted to use. For example, someone allowed in the production team area but not allowed in the research and development area.

| Command | Description | Example | Notes |
|---------|-------------|---------|-------|
| `az ad user list` | List all Entra ID users | `az ad user list --output table` | Use `--filter` to search by department, job title, location, or subscription so that you don't need to scroll through thousands of users to find the correct one. |
| `az ad user create` | Create a new Entra ID user | `az ad user create --display-name "Scott Rayner" --password "P@ssw0rd!" --user-principal-name "scott@domain.com"` | Password complexity is enforced to avoid easier brute force attacks against weak passwords like password123 or 12345678. |
| `az ad user delete` | Delete an Entra ID user | `az ad user delete --id "scott@domain.com"` | User Principal Name (UPN) is the username and domain suffix which makes it easier for the human to interpret, such as scott@domain.com. The object ID is for the computer to understand and ensure all users are unique via a unique code. |
| `az ad group list` | List all Entra ID groups | `az ad group list --output table` | |
| `az ad group create` | Create a new Entra ID group | `az ad group create --display-name "Admins" --mail-nickname "Admins"` | Creating a group in Azure is to group users into a team such as development or production. This makes it easier and quicker to assign and manage permissions at scale. It also makes it simpler to revoke access when a user leaves, by simply removing them from the group rather than adjusting individual permissions. |
| `az ad group member add` | Add a user to a group | `az ad group member add --group "Admins" --member-id "<object-id>"` | Used when a new user joins a team or moves from another department. The user will inherit all permissions assigned to the group, ensuring consistent access without needing to configure permissions individually. |
| `az role definition list` | List all available RBAC roles | `az role definition list --output table` | |
| `az role assignment create` | Assign an RBAC role to a user | `az role assignment create --assignee "scott@domain.com" --role "Contributor" --scope "/subscriptions/<sub-id>"` | Role assignment creates access control through three core categories. The Owner has full day to day control similar to a lecturer, the Contributor can create and manage resources but cannot grant access to others similar to an Internal Quality Assurer (IQA) who checks and verifies work, and the Reader can only view resources similar to an External Quality Assurer (EQA) who observes and reviews without making changes. Access can be permanently assigned or temporarily granted through Privileged Identity Management (PIM). |
| `az role assignment list` | List role assignments | `az role assignment list --output table` | Add `--all` to include inherited roles |
| `az role assignment delete` | Remove a role assignment | `az role assignment delete --assignee "scott@domain.com" --role "Contributor"` | |
| `az policy definition create` | Create a custom policy definition | `az policy definition create --name "deny-untagged" --rules policy.json` | A policy definition is a rule that can be applied to a resource or a group and has three different effects. Deny ensures that the action cannot be carried out at all, like restricting equipment use behind locked cabinets with access only to those that are trained. Modify fixes issues automatically like adding missing tags to a resource. Audit logs the action with a warning to be reviewed by administrators. |
| `az policy assignment create` | Assign a policy to a scope | `az policy assignment create --name "deny-untagged-assign" --policy "deny-untagged" --scope "/subscriptions/<sub-id>"` | Assigning a policy to a resource or group is the difference between writing a set of rules and applying them. Applying them ensures that the rules are enforced within the required scope and restrictions. |
| `az policy assignment list` | List all policy assignments | `az policy assignment list --output table` | |
| `az policy assignment delete` | Remove a policy assignment | `az policy assignment delete --name "deny-untagged-assign"` | |
| `az account management-group list` | List all management groups | `az account management-group list --output table` | |
| `az account management-group create` | Create a management group | `az account management-group create --name "mg-production"` | Creating a management group oversees a set of subscriptions and their corresponding resource groups. Similar to a company hierarchy where the area manager (management group) oversees the department managers (subscriptions), who oversee the departments (resource groups), who are a collective of specialists (resources). |

---

