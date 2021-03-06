---
group: cloud-guide
title: Create and manage users
functional_areas:
  - Cloud
  - Configuration
redirect_from:
  - /cloud/admin/admin-user-admin.html
---

You can manage user access to {{site.data.var.ece}} projects by assigning users one or more roles. You can add and manage user accounts for the entire project and permissions per available environment.

{:.bs-callout-tip}
Adding or updating user account information for a {{site.data.var.ece}} environment triggers the build and deploy process automatically, which takes your site offline until the deployment completes. For Production environments, we recommend completing user account management tasks during off-peak hours to prevent service disruptions.

## Account owner role {#cloud-role-acct-owner}

The License Owner is the only user with the Account Owner role. This user can perform any task in any project or environment, including deleting it. The account is associated with the email address, name, and information for the person who registered the {{site.data.var.ece}} account through the account creation process.

The account has super user access and additional capabilities for managing all aspects of your project and environments.

## Project-level roles {#cloud-role-project}

You can assign the following project-level roles to users:

-  The **Super user** role grants administrator access to all environments. They can change settings and execute actions on any environment, including creating and restoring snapshots.
-  The **Project reader** role grants view access to all environments in a project. Users with this role cannot execute actions on any environment.

## Environment-level roles {#cloud-role-env}

A project reader can have one of the following roles per environment:

-  The **Admin** role grants access to change settings and execute actions on an environment, including merging with the parent environment.
-  The **Contributor** role grants access to push code to an environment and branch the environment.
-  The **Reader** role, also referred to as the _viewer_ role grants view-only access to an environment.

## Role management best practices

-  We recommend that you limit the project Super user role and environment Admin roles to as few users as possible.

-  When a development team works on a project, the team leader can be the project administrator who decides which roles to assign to team members. For example, the team lead might assign one team member as a Contributor to one environment, assign another as an Admin on a different environment, and assign the Reader role to the customer on the `master` environment.`

-  Assign the Contributor role to users who require view access to an environment as well as the capability to commit code and branch the environment.

{:.bs-callout-warning}
An environment contributor can push code to the environment, but that user role does not have SSH access to the environment. By default, only environment administrators have SSH access. You can change this behavior by updating the access configuration in the `.magento.app.yaml` file to include `ssh: contributor`.

## Create and manage users

You can create and manage users using the Magento Cloud CLI or the {{site.data.var.ece}} Project Web UI.

### Manage users with the CLI {#cloud-user-mg-cli}

You can use the {{site.data.var.ece}} command line client to manage users and integrate this with any other automated system.

Available commands:

-  `magento-cloud user:add`–add a user to the project
-  `magento-cloud user:delete`–delete a user
-  `magento-cloud user:list [users]`–list project users
-  `magento-cloud user:role`–view or change the user role

The following examples show how to add a user and configure the project and environment-level role, and how to how to modify project assignments and assigned user roles.

{:.procedure}
To add a user and assign roles:

1. Add the user:

   ```bash
   magento-cloud user:add
   ```

1. Follow the prompts to specify the user email address, set the project and environment roles, and add the user:

   ```terminal
   Enter the user's email address: alice@example.com

   Email address: alice@example.com

   The user's project role can be 'viewer' ('v') or 'admin' ('a').
   Project role [V/a]: a
   The user's environment-level roles can be 'viewer', 'contributor', or 'admin'.
   development environment role [V/c/a]: c
   Summary:
     Email address: alice@example.com
     Project role: contributor
   Adding users can result in additional charges.
   Are you sure you want to add this user? [Y/n]
   Adding the user to the project
   ```
   {:.no-copy}

   {%include cloud/note-prevent-site-availability-issues.md%}

   After you add the user, Magento sends an email to the specified address with instructions for accessing the {{ site.data.var.ece }} project.

The following example changes the environment-level role that is assigned to a user:

```bash
magento-cloud user:role alice@example.com --level environment --environment development --role admin
```

{:.bs-callout-info}
To list the available `magento-cloud` CLI commands, use the `magento-cloud list` command.

### Manage users from the Project Web UI {#cloud-user-webinterface}

{:.procedure}
To create user accounts from the Project Web UI:

1. Log in to [your {{site.data.var.ece}} account](https://accounts.magento.cloud).

1. Click the **Projects** tab as the following figure shows.

   ![Click the projects tab to access your Cloud project]({{ site.baseurl }}/common/images/cloud_account_project.png){:width="550px"}

1. Click the name of your project.

1. Click the configure project button next to project name in the top navigation bar as the following figure shows.

   ![Configure the project]({{ site.baseurl }}/common/images/cloud_project_gear.png){:width="184px"}

1. In the right pane, click **Add Users**.

   ![Start creating users]({{ site.baseurl }}/common/images/cloud_project-config.png){:width="500px"}

1. Click **Add User**.

   ![Add users]({{ site.baseurl }}/common/images/cloud_project-add-superuser.png){:width="500px"}

1. Enter the user e-mail address.

1. Select the access for the account:

   -  For a project administrator account, select the **Super User** checkbox. This provides Admin rights for all settings and environments. If not selected, the account has only view options for all project environments.

   -  Select permissions per specific environment (or branch) in the Integration environment: _No access_, _Admin_ (change settings, execute action, merge code), _Contributor_ (push code), or _Reader_ (view only). When you add active environments, you can modify permissions per user.

1. Click **Add User**.

   {%include cloud/note-prevent-site-availability-issues.md%}

The user you add receives an email inviting them to join the {{site.data.var.ece}} project with instructions for account registration and email verification.

