# Azure PowerShell Command Reference
> Azure Administrator (AZ-104) — Command reference for all topic areas

Azure PowerShell is a scripting language specifically for Microsoft admins and engineers as it is fully integrated within the Microsoft and Windows ecosystems. It uses a Verb-Noun structure rather than Linux style syntax. Both use an Application Programming Interface (API) to send their structured commands to Azure.

---

## How to Read This Document

PowerShell's structure is conformative in the sense that it is a verb followed by a noun, meaning that it is like 'get this from the policy assignment'. A couple of things to note which are required within the command are the hyphen and Az between the verb-noun structure — the hyphen is required to join the verb-noun into a single unified command and the Az is to tell PowerShell that it relates to Azure.

| Part | What it means | Example |
|------|---------------|---------|
| Verb | What you want to do | `Get`, `New`, `Set`, `Remove` |
| `-` | Joins the command into one | `Get-` |
| `Az` | Tells PowerShell this is an Azure command | `Get-Az` |
| Noun | What you are targeting in Azure | `Get-AzVM`, `Get-AzPolicyAssignment` |

**Full example:** `New-AzResourceGroup -Name "rg-demo" -Location "uksouth"`

---

## Parameter Syntax

PowerShell unlike CLI uses a single dash to unify the command into a single construct, taking the verb followed by the hyphen followed by the noun. This keeps each command simple and easier to read when compared to CLI syntax. For parameters, the dash precedes the parameter name to ensure PowerShell knows the specific details to apply to the command, for example resource, location, and group.

| Syntax Rule | Example |
|-------------|---------|
| Verb-Noun joined by a hyphen | `Get-AzVM`, `New-AzResourceGroup` |
| Parameters use a single dash | `-Name`, `-Location`, `-ResourceGroupName` |
| Value follows with a space | `-Location "uksouth"` |
| Strings with spaces need quotes | `-Name "my resource group"` |
| Strings without spaces do not | `-Location uksouth` |

---

## Getting Started

One difference compared to CLI is that if you have not already done so, you are required to install the Az module into PowerShell before you can connect remotely. Unlike CLI which uses a login command, PowerShell uses the term Connect to establish the same connection, similar to logging into the web portal but through a command rather than clicking an empty box to fill in your credentials. Once connected you would select your subscription and whether you are going into development or production, similar to first and second fix,  where first fix is the staging area, installing the cabling and accessories, before production in second fix ensuring the testing is completed before going live.

| Action | Command |
|--------|---------|
| Install the Az module (one-time setup) | `Install-Module -Name Az -AllowClobber -Force` |
| Connect to Azure | `Connect-AzAccount` |
| List all subscriptions | `Get-AzSubscription` |
| Show current active subscription | `Get-AzContext` |
| Set active subscription | `Set-AzContext -SubscriptionId "your-subscription-id"` |

---

## 1. Identity & Governance

Identity and Governance is all about who people are, what their roles are, and the areas they can access. Similar to an office where an ID card is the identification but also has a chip for location access, the commands in Azure are similar to the security checks of the ID card when entering a building or area. The main purpose of these commands is to assign roles and responsibilities to individuals or groups, ensuring that someone cannot gain access to a service they are not permitted to use — for example, someone allowed in the production team area but not allowed in the research and development area.

| Command | Description | Example | Notes |
|---------|-------------|---------|-------|
| `Get-AzADUser` | List all Entra ID users in your tenant | `Get-AzADUser` | Use `-Filter` to search by department, job title, location, or subscription so that you don't need to scroll through thousands of users to find the correct one. |
| `New-AzADUser` | Create a new Entra ID user | `New-AzADUser -DisplayName "Scott Rayner" -Password "P@ssw0rd!" -UserPrincipalName "scott@domain.com"` | Password complexity is enforced to avoid easier brute force attacks against weak passwords like password123 or 12345678. |
| `Remove-AzADUser` | Delete an Entra ID user | `Remove-AzADUser -UserPrincipalName "scott@domain.com"` | User Principal Name (UPN) is the username and domain suffix which makes it easier for the human to interpret, such as scott@domain.com. The object ID is for the computer to understand and ensure all users are unique via a unique code. |
| `Get-AzADGroup` | List all Entra ID groups | `Get-AzADGroup` | |
| `New-AzADGroup` | Create a new Entra ID group | `New-AzADGroup -DisplayName "Admins" -MailNickname "Admins"` | Creating a group in Azure is to group users into a team such as development or production. This makes it easier and quicker to assign and manage permissions at scale. It also makes it simpler to revoke access when a user leaves, by simply removing them from the group rather than adjusting individual permissions. |
| `Add-AzADGroupMember` | Add a user to a group | `Add-AzADGroupMember -GroupDisplayName "Admins" -MemberUserPrincipalName "scott@domain.com"` | Used when a new user joins a team or moves from another department. The user will inherit all permissions assigned to the group, ensuring consistent access without needing to configure permissions individually. |
| `Get-AzRoleDefinition` | List all available Role-Based Access Control (RBAC) roles | `Get-AzRoleDefinition` | |
| `New-AzRoleAssignment` | Assign an RBAC role to a user | `New-AzRoleAssignment -SignInName "scott@domain.com" -RoleDefinitionName "Contributor" -Scope "/subscriptions/<sub-id>"` | Role assignment creates access control through three core categories. The Owner has full day to day control similar to a lecturer, the Contributor can create and manage resources but cannot grant access to others similar to an Internal Quality Assurer (IQA) who checks and verifies work, and the Reader can only view resources similar to an External Quality Assurer (EQA) who observes and reviews without making changes. Access can be permanently assigned or temporarily granted through Privileged Identity Management (PIM). |
| `Get-AzRoleAssignment` | List all role assignments | `Get-AzRoleAssignment` | |
| `Remove-AzRoleAssignment` | Remove a role assignment | `Remove-AzRoleAssignment -SignInName "scott@domain.com" -RoleDefinitionName "Contributor"` | |
| `New-AzPolicyDefinition` | Create a custom policy definition | `New-AzPolicyDefinition -Name "deny-untagged" -Policy "policy.json"` | A policy definition is a rule that can be applied to a resource or a group and has three different effects. Deny ensures that the action cannot be carried out at all, like restricting equipment use behind locked cabinets with access only to those that are trained. Modify fixes issues automatically like adding missing tags to a resource. Audit logs the action with a warning to be reviewed by administrators. |
| `New-AzPolicyAssignment` | Assign a policy to a scope | `New-AzPolicyAssignment -Name "deny-untagged-assign" -PolicyDefinition (Get-AzPolicyDefinition -Name "deny-untagged") -Scope "/subscriptions/<sub-id>"` | Assigning a policy to a resource or group is the difference between writing a set of rules and applying them. Applying them ensures that the rules are enforced within the required scope and restrictions. |
| `Get-AzPolicyAssignment` | List all policy assignments | `Get-AzPolicyAssignment` | |
| `Remove-AzPolicyAssignment` | Remove a policy assignment | `Remove-AzPolicyAssignment -Name "deny-untagged-assign"` | |
| `Get-AzManagementGroup` | List all management groups | `Get-AzManagementGroup` | |
| `New-AzManagementGroup` | Create a management group | `New-AzManagementGroup -GroupName "mg-production"` | Creating a management group oversees a set of subscriptions and their corresponding resource groups. Similar to a company hierarchy where the area manager (management group) oversees the department managers (subscriptions), who oversee the departments (resource groups), who are a collective of specialists (resources). |

---
