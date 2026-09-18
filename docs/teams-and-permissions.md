# Teams and permissions
## 

Access is granted to teams rather than to individual users. A member's permissions are the highest access level of all their teams.

### Access levels

- **Owner**: everything, including billing and the [owner-only actions](#owner-only-actions).
- **Admin**: all packages, teams, members and settings. Billing only when added to the _Billing_ team.
- **Edit packages**: change the settings of the team's packages, delete them, give their own other teams access to them, and install them with Composer.
- **Read packages**: see the team's packages and install them with Composer.
- **Billing**: billing pages only, no packages. Members who are only in the _Billing_ team do not count towards your subscription usage.

Only the built-in _Owners_ and _Admins_ teams grant owner and admin access. Every other team reads or edits packages.

### Built-in teams

Four teams cannot be renamed or deleted:

- _Owners_, _Admins_ and _Billing_ grant the access levels above and have no settings.
- _All Organization Members_ contains every member automatically, its membership cannot be edited. Give it access to a package to give everyone access.

### Team permissions

Independent of the access level, a team's settings page grants:

- _Global Package Permissions_: add packages, credentials and mirrored third-party repositories.
- _Suborganization Permissions_: create suborganizations.
- _Customer Permissions_: view or manage customers, for Private Packagist for Vendors.

Owners and admins always have all of these.

### Managing teams and members

#### With a synchronization

Your code hosting platform is the source:

- The admins and owners of the primary synchronization's remote organization become the members of _Admins_ and _Owners_. Promote people there. Additional synchronizations grant no admin or owner access.
- Remote teams are created with read access and keep their remote name.
- Teams cannot be created or deleted, and members cannot be added or removed, neither in the web interface nor through the API.

Owners and admins still manage:

- The _Billing_ team, so billing access needs no account on your code hosting platform. It is the only team you can still invite people to by email.
- Deactivating a member on the _Members_ page. They lose access and no longer count towards your subscription, and their team memberships return when you reactivate them. New members a synchronization discovers can be deactivated by default.
- Every team's access level, permissions and package access, synchronized teams included. Packages that are part of the synchronization are excluded.

See [Synchronizations](synchronizations-faq).

#### Without a synchronization

- Owners and admins create teams and let them read or edit packages.
- They add members to teams and invite new users by email. This includes the _Admins_ and _Billing_ teams, so an admin can make another user an admin.
- They remove members from the organization on the _Members_ page.
- Only owners add members to _Owners_ or remove them.
- The [REST API](api/overview) manages teams and their members, except _Owners_. Inviting, removing and deactivating people in the organization is web interface only.

### The Owners team

Owners cannot be locked out of their own organization:

- Admins cannot remove or deactivate an owner.
- The last owner is never removed, not even by a synchronization.

Keep at least two owners: only owners can reset another member's multi-factor authentication.

### Owner-only actions

- Managing the _Owners_ team.
- Billing: subscription, payment information, invoices. The _Billing_ team has access too.
- Resetting another member's multi-factor authentication, if that member belongs to no other organization.
- Deactivating and deleting the organization. On Self-Hosted an instance administrator can disable deletion.

### Suborganizations

Permissions work through teams as well. Add a team to a suborganization and its members can access every package in it. External collaborators can be invited to a single suborganization without joining your organization. See [Suborganization setup](setup-suborganization).
