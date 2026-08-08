> ## Documentation Index
> Fetch the complete documentation index at: https://docs.retellai.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Create and manage Retell workspaces

> Create and manage Retell workspaces, find your workspace ID, invite teammates to collaborate, switch between workspaces, and safely delete a workspace.

Learn how to create and manage workspaces, and collaborate with team members.

## Default Workspace

When you first create an account, a default workspace is automatically created for you. This workspace serves as your primary environment for managing projects and collaborating with team members.

## Find your Workspace id / org id

Sometimes we might ask for your workspace id / org id when debugging issues related to your account. You can find it in your workspace settings page.

Open **Settings** from the sidebar:

<Frame caption="Settings in the sidebar">
  <img height="700" src="https://mintcdn.com/retellai/BOVC_TBrgLAZfOvP/accounts/images/settings.png?fit=max&auto=format&n=BOVC_TBrgLAZfOvP&q=85&s=86ede48642d2e58e65cf59f1b322ac36" alt="Retell sidebar with Settings highlighted under System" data-path="accounts/images/settings.png" />
</Frame>

Then open **Workspace** → **General**. Your workspace ID appears under **Workspace ID**:

<Frame caption="Workspace ID in General settings">
  <img height="700" src="https://mintcdn.com/retellai/BOVC_TBrgLAZfOvP/accounts/images/workspace_settings.png?fit=max&auto=format&n=BOVC_TBrgLAZfOvP&q=85&s=dd94d96e39c40cd4e08fbe13e784345e" alt="Workspace General settings showing Workspace Name and Workspace ID" data-path="accounts/images/workspace_settings.png" />
</Frame>

## Creating Additional Workspaces

To create a new workspace:

1. Click the workspace selector in the top left corner of the dashboard
2. Select **Add another workspace**
3. Enter your workspace name
4. Click **Save**

<Frame caption="Workspace selector with Add another workspace">
  <img height="700" src="https://mintcdn.com/retellai/BOVC_TBrgLAZfOvP/accounts/images/add_workspace.png?fit=max&auto=format&n=BOVC_TBrgLAZfOvP&q=85&s=59b9ab9cc7e708f30f2e0d0cb2b82e71" alt="Workspace dropdown with Add another workspace highlighted" data-path="accounts/images/add_workspace.png" />
</Frame>

## Managing Team Members

### Inviting Members

To invite team members to your workspace:

1. Open **Settings** → **Workspace** → **Users**
2. Click **Invite a member**
3. Enter their email address and select a role
4. Send the invitation

If a team member does not receive the invitation email, you can resend it by inviting them again with the same email address.

<Frame caption="Users list">
  <img height="700" src="https://mintcdn.com/retellai/BOVC_TBrgLAZfOvP/accounts/images/users.png?fit=max&auto=format&n=BOVC_TBrgLAZfOvP&q=85&s=0487749d1aa49bb07b6ddf8e9b4ceb7b" alt="Workspace Users page with Invite a member button and member list" data-path="accounts/images/users.png" />
</Frame>

<Frame caption="Invite a member dialog">
  <img height="700" src="https://mintcdn.com/retellai/BOVC_TBrgLAZfOvP/accounts/images/invite_members.png?fit=max&auto=format&n=BOVC_TBrgLAZfOvP&q=85&s=b159a88710cea935d92e3890f0aac977" alt="Invite a member dialog with email field and role options Admin, Developer, and Member" data-path="accounts/images/invite_members.png" />
</Frame>

### Invitation Process

* Invited members receive an email with a link to join the workspace
* They can create a new account or use an existing one
* Once accepted, they have immediate access

<Frame caption="Invitation email">
  <img height="700" src="https://mintcdn.com/retellai/BOVC_TBrgLAZfOvP/accounts/images/invite_email.png?fit=max&auto=format&n=BOVC_TBrgLAZfOvP&q=85&s=c7670b93d5dc9ce8012a17b1ed1dbe2d" alt="Email invitation to join a Retell AI workspace with Accept Invitation button" data-path="accounts/images/invite_email.png" />
</Frame>

## Leave a Workspace

To leave a workspace:

1. Open **Settings** → **Workspace** → **Users**
2. Click **Leave Workspace**
3. Confirm your action

**Important considerations before leaving:**

* You can only leave a workspace if there is at least one other member remaining
* If you are the only member in the workspace, you must delete the workspace instead
* You can rejoin the workspace if another member invites you back

<Frame caption="Leave Workspace on the Users page">
  <img height="700" src="https://mintcdn.com/retellai/BOVC_TBrgLAZfOvP/accounts/images/leave_workspace_settings.png?fit=max&auto=format&n=BOVC_TBrgLAZfOvP&q=85&s=ee216c375ca1d0a9ba885204656a1baa" alt="Users page with Leave Workspace button" data-path="accounts/images/leave_workspace_settings.png" />
</Frame>

## Delete Workspace

In **Settings** → **Workspace** → **General**, open the options menu (⋯) and select **Delete**. Before deletion, please note:

* All workspace data will be permanently deleted and cannot be recovered
* A final invoice will be generated and charged for any usage up to the deletion date
* All team members will lose access to the workspace
* Any active API keys will be invalidated

<Note>
  If there is no payment method on file when you delete the workspace, the final invoice is still generated for any outstanding usage and remains due. Add a valid payment method before deleting (see [Add payment methods](/accounts/add-payment)) so the final charge can settle automatically. If a charge fails afterward, follow [Handle failed payments](/accounts/fail-payment) or contact Retell support to resolve the balance.
</Note>

<Frame caption="Delete option in General settings">
  <img height="700" src="https://mintcdn.com/retellai/BOVC_TBrgLAZfOvP/accounts/images/delete_workspace.png?fit=max&auto=format&n=BOVC_TBrgLAZfOvP&q=85&s=d295896513b4da9eeb90aa3dbe70f884" alt="Workspace General settings with Delete option in the options menu" data-path="accounts/images/delete_workspace.png" />
</Frame>

<Frame caption="Delete workspace confirmation">
  <img height="700" src="https://mintcdn.com/retellai/BOVC_TBrgLAZfOvP/accounts/images/delete_workspace_popup.png?fit=max&auto=format&n=BOVC_TBrgLAZfOvP&q=85&s=a2c7b43ff50bca86dbac47cc4cbbdad1" alt="Confirmation dialog requiring the workspace name before deleting" data-path="accounts/images/delete_workspace_popup.png" />
</Frame>
