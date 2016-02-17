# How can I add or remove users with my organization?

## Adding Users to an Organization

Adding users to an organization is managed through an invitation process controlled via the dashboard or the CLI.

### Dashboard User Invites

Log into the [Stratum Dashboard](https://stratum.catalyze.io) with an admin or owner account for the organization that you want to manage.

At the top of the screen, mouse over the `Organizations` button and select the organization to manage.

![org_dropdown](images/organization_dropdown.png)

Clicking on the organization brings up the overview display for the organization. On the right side of the screen is the `Invites` panel. To invite someone to an organization, click the `Add User` button. Then, type in their e-mail address, select a role for them, and invite them.

![org_invite](images/organization_invite.png)

Their invitation will be listed as pending below the invite box until the user accepts the invitation.

![org_invite_pending](images/organization_invite_pending.png)

Once the user accepts the invitation, they will show up in the organization user list and an admin or owner can authorize their environment access.

### CLI User Invites

Follow the CLI reference documentation [here](https://resources.catalyze.io/paas/paas-cli-reference/invites)

## Removing Users from an Organization

### Dashboard User Removal

Log into the [Stratum Dashboard](https://stratum.catalyze.io) with an admin or owner account for the organization that you want to manage.

At the top of the screen, mouse over the `Organizations` button and select the organization to manage.

![org_dropdown](images/organization_dropdown.png)

In the list of organization users, click the red "X" next to the user you wish to remove:

![user_delete](images/dashboard_user_delete.png)

### CLI User Removal

Follow the CLI reference documentation [here](https://resources.catalyze.io/paas/paas-cli-reference/invites)
