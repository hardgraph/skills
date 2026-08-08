> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Access Control

> Manage your Retell workspace with role-based access control: invite members, assign Admin, Developer, Analyst, or Viewer roles, and control permissions.

To keep your workspace safe, we have an RBAC (Role-Based Access Control) system.

### System Roles

#### Admin

Full control over workspace resources and members. Complete access to all features including billing, user management, and workspace settings.

Can:

* Invite, remove, and change roles for members
* View and manage billing: usage, invoices, payment methods, subscriptions, and customer portal
* Create, edit, and delete agents, conversation flows, Retell LLMs, knowledge bases, voices, and folders
* Configure developer settings
* Access all data, including raw transcripts and recordings
* Manage telephony settings
* Update or delete the workspace

Cannot:

* None — Admins have full permissions

#### Developer

Full functional access to build and test agents, view raw data, manage analytics and developer settings. Cannot manage billing or organization users.

Can:

* Build and edit agents, flows, LLMs, KBs, voices
* Test and simulate; manage test cases and playground
* View raw logs, transcripts, recordings, and analytics
* Create exports; run calls and batch calls
* Manage API/public keys and webhooks; adjust concurrency/CPS

Cannot:

* Manage billing (usage, invoices, payment methods, subscriptions)
* Invite, remove, or change member roles
* Update or delete the workspace

#### Member

Read-only access to agents, testing artifacts, scrubbed history, and analytics. Cannot make changes or view sensitive data.

Can:

* View agents and configurations (read‑only)
* View tests, playground threads, and analytics
* View scrubbed history; batch calls and phone resources
* List workspace members

Cannot:

* Create, edit, or delete resources
* Start calls/chats, run simulations, or change playground
* Access API/public keys, webhooks, or raw transcripts/recordings
* Manage billing, settings, or team members

### Invite User

Now under the user management of your workspace, you (admin) can invite a user with a specific role.

<Frame caption="Invite user to workspace">
  <img height="700" src="https://mintcdn.com/retellai/1CfiInukE0_eM7O7/images/invite_user.jpeg?fit=max&auto=format&n=1CfiInukE0_eM7O7&q=85&s=ac2463d85c7db215ff309f553ddec6a5" alt="invite_user.jpeg" data-path="images/invite_user.jpeg" />
</Frame>

### Change User Role

You can change the role of active users in the workspace.

<Frame caption="Change user role">
  <img height="700" src="https://mintcdn.com/retellai/ql-QUtpW4EfxaGjq/images/update_user_role.jpeg?fit=max&auto=format&n=ql-QUtpW4EfxaGjq&q=85&s=d3a70efab1471b8ab0421dc57c9b3dc0" alt="update_user_role.jpeg" data-path="images/update_user_role.jpeg" />
</Frame>

### Remove User

You can also remove a user from your workspace.
