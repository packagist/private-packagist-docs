# Changelog
## Private Packagist Enterprise

#### 1.6.0
*August 20, 2018*

**Features**
- Email/password based registration and login, needs to be enabled on /admin/ page under Global Settings
- Email verification configurable per integration to set trust for emails provided by third party authentication mechanisms
- Improved profile page with list of emails from third parties and manually defined ones, each with verification state and option to use as default address
- Display README contents on package overview pages

**Changes**
- If a user's API rate limit on a third party service prevents login, display a specific error message explaning this circumstance
- Team overview page lists package counts next to permissions now, so it's easier to spot teams which assign permissions to an incomplete set of packages
- Improve Enterprise setup experience by automatically granting the first created user admin permissions
- Automatically fill in bitbucket usernames in credential creation forms to simplify setup
- Improved documentation layout and added enterprise troubleshooting section

**Bugfixes**
- Convert proxy URLs to correct protocol URLs for PHP streams
- Automatically send full URIs to HTTP proxies for unencrypted HTTP, but relative paths for encrypted HTTPS to avoid problems with some specific proxy software
- Revert modified user agent in proxy code which preventing mirroring from third party repositories blocking based on user agents
- Disabled erroneously enabled but broken vendor/customer functionality on Private Packagist Enterprise

#### 1.5.7
*August 7, 2018*

**Features**
- Respect the host system's PROXY environment variables which are used by replicated inside of Private Packagist and allow the definition of hostnames which should not go through the proxy on the admin page
- Added an option to change the frequency of organization synchronization below the default 20 hours, watch out for rate limit issues if you reduce this value
- Provide a CLI admin command to update all packages in an organization
- Archives for the latest tages of new packages are now created immediately to speed up the first use of composer update
- add missing links to profile page to allow editing username and email easily
- Composer token last usage is now tracked precise to a 5 minute window

**Changes**
- Improved validation of Enterprise Integration setup values for GitHub Enterprise, Bitbucket Server and self-hosted GitLab with improved error messages
- Provide a more specific error message if a package already exists when you are adding it with a link to the package
- Use streaming responses for large JSON files to reduce memory consumption
- Upgraded Replicated to version 2.25.1
- Increased timeout for mirrored repository responses to 5 seconds to allow slower repositories to be mirrored, may slow down composer updates when adding new packages
- Reduced default php socket timeout to 10 seconds to provide useful error messages if any external requests time out
- moved automatic webhook creation into a worker process with automatic retries to improve reliability

**Bugfixes**
- Do not store URLs to Private Packagist in cache, so hostname changes take effect without manual regeneration of stored data
- Synchronization of an organization with GitHub no longer stops if a repository with individual collaborators is archives
- Display a link using custom integration domains for creating credentials instead of always using github.com, gitlab.com, bitbucket.org
- metapackages without code no longer trigger archive creation errors
- License overview no longer displays package names to a user which they do not have access to
- GitLab scope missing error is now explained to users instead of crashing background workers
- Fix internal communication between containers for SSL certificates which are not valid for IP addresses used on the internal network, could have resulted in empty download statistics
- Fix internal SSL communication between containers when using Replicated's self signed certificate
- Fix worker crash if synchronization setup is disabled while synchronization is running
- Validate informational domain field for credentials

#### 1.5.6
*Release skipped*

#### 1.5.5
*Release skipped*

#### 1.5.4
*June 7, 2018*

**Features**
- Let users know that they have to accept GitLab's new ToS if this is preventing them from logging in or synchronizing their organization

**Changes**
- Automatically generated credential names are no longer unique across all orgsnizations
- GitHub push hooks will trigger initialization of packages that do not exist yet
- Catch GitLab API Limit errors and display a message to users
- Reduce amount of data requested from GitLab by using new subgroup API endpoints if available as of GitLab 10.3
- During successful synchronization the user who set up the synchronization will not be removed from owners if no other owner has an active Private Packagist account

**Bugfixes**
- Package updates triggered by a push hook are retried after 30 seconds instead of being cancelled, if another update is already running, as a result tags pushed separately immediately after commits should now show up on Packagist faster
- Regression preventing successful synchronization with GitHub Enterprise
- Warn users about problems if gitlab sync credential is empty
- Handle Bitbucket 50x errors the same as 500 during synchronization API calls
- Attempt to retrieve smaller pieces of information with more API calls from Bitbucket if large endpoints return 500 errors for internal timeouts at Bitbucket
- Catch 401 errors from GitLab and turn into useful message for users
- Enterprise preflight checks test listening on 0.0.0.0 for ports 80/443 now to avoid issues with machines without a detectable public ip
- Custom package validation now properly checks that at least one package was defined

#### 1.5.3
*May 9, 2018*

**Bugfixes**
- Fix installation of webhooks on Bitbucket Server if no webhooks are present yet

#### 1.5.2
*May 8, 2018*

**Bugfixes**
- Repo container won't start up if restarted without old temporary directory present

#### 1.5.1
*May 8, 2018*

**Features**
- Automatically configure webhooks on Bitbucket Server (requires version 5.4 or above)

**Changes**
- Reorganized organization teams page layout
- Separate packagist temporary data from permanent distribution files to decrease snapshot size, we recommend you use SFTP or S3 snapshots only and that you define a new empty directory to create snapshots in starting with this release to reset the differential storage mechanism
- Composer repository now returns a 401 if auth is missing to ask for interactive authentication input
- Upgraded to Replicated 2.20.2 with improvements to snapshot creation and restore

**Bugfixes**
- Fix key generation for Bitbucket Server and provide more detailed error messages in case of problems
- Ensure packages with manually defined JSON always update all metadata on edits
- Detect and ignore deleted package situation when processing a package update
- Prevent signup errors if people double click the signup button
- Catch 502 responses from GitLab and treat them as timeouts and retry later
- Catch empty responses from Bitbucket and treat them as timeouts and retry later
- If bitbucket permission API endpoint fails, try retrieving info for each team and repository individually
- Use correct organization name for listed vcs repositories if an org is synchronized with multiple targets
- Fix audit log entry creation for admin self-deletion
- Ensure synchronized users are always created with a unique username
- Find composer.json information on GitLab projects with a default branch other than master
- Correctly disable all synchronizations if an integration on which they are based is deleted

#### 1.5.0
*Mar 16, 2018*

**Features**
- Management console option to enter additional TLS/SSL certificates Private Packagist Enterprise should trust for connections to your VCS repositories
- Option to trigger an update for all packages mirrored from a particular third party Composer repository
- GitHub synchronization now supports granting external collaborators package permissions
- Synchronization now automatically creates packages from repositories which you add a composer.json to after sync skipped the repository for lack of a composer.json

**Changes**
- Restoring an Enterprise instance from a snapshot no longer requires to run any custom commands to restore the PostgreSQL database
- If a GitLab subgroup is synchronized with a Private Packagist organization, we now search all parent groups for members with inherited permissions
- Increased default timeout for talking to GitLab instances to 30 seconds
- Reaching the Bitbucket rate limit now triggers a 1 hour block on all requests sent to the Bitbucket API to restore the limit fully
- Bitbucket requests are retried automatically on server errors because it reports timeouts as 500 Internal Server Error
- Performance improvements to Composer repository JSON delivery
- Provide a specific error message if someone attempts to add a package with the same name as a package that already exists
- Setup mode now allows deleting the admin user account
- Disabled auto-complete on password fields for external credentials
- Upgrade to Replicated 2.17.0 amongst other things improving snapshots and restore

**Bugfixes**
- Worker processes do not fail to start up anymore if Redis is taking too long to load data into memory
- Modified Redis backup/restore process will no longer cause extremely slow or incomplete snapshots
- Fix GitLab Composer behavior to ensure correct commit instead of branch is used for dist generation when targeting specific commits on branches
- Updating the credentials on a mirrored Composer repository now ensures that all packages already mirrored from the same repository start using the updated credentials
- Bitbucket Server configuration uses readonly instead of disabled fields for values to be copied now and made it impossible to reset client id / public key to empty
- Do not log anything and do not return an error on metapackages which do not have a source or dist specified
- Invitation email to new users now links to correct login/registration page instead of always refering to GitHub
- Fix local build file cleanup to prevent disk from eventually getting too full
- Fix mirroring of packages with names using capital letters (should be avoided and is invalid on packagist.org)
- Prevent error on editing an organization if no platform was selected for the organization

#### 1.4.3
*Jan 17, 2018*

**Changes**
- Synchronized packages will always use the credentials associated with the sync now

**Bugfixes**
- Do not attempt to bind to [::] on Enterprise hosts with IPv6 disabled
- Skip synchronization entirely for organizations that no longer exist on GitHub
- Fix webhook installation on non-primary synchronizations
- Properly apply deactivated user list to non-primary synchronizations

#### 1.4.2
*Jan 10, 2018*

**Bugfixes**
- Fix form submission for secondary GitHub synchronization default permission settings

#### 1.4.1
*Jan 10, 2018*

**Bugfixes**
- Fix regression on some package pages and in background workers attempting to resolve non-existent default integration configuration leading to 500 errors
- Fix incorrect determination of mailer host configuration resulting in broken health checks

#### 1.4.0
*Jan 8, 2018*

**Features**
- Synchronize multiple external GitHub orgs, Bitbucket teams, GitLab groups with a single Private Packagist organization
- Multiple organizations can be synchronized with the same GitHub/Bitbucket/GitLab organization
- Allow disabling the creation of new organizations for anyone who is not a Private Packagist Admin user

**Changes**
- Synchronized team names have been updated to reflect their origin to clarify context when using synchronization with multiple sources
- Do not allow deleting an organization on Private Packagist if the GitHub app has not been uninstalled yet
- Improved 500 error message on Enterprise installations
- Upgraded Replicated to 2.15.0
- Upgraded Nginx to 1.13.8

**Bugfixes**
- Remove non-existent option from worker command calls, fixing issues with cache invalidation and download stats
- Fix potential constraint violation when disabling synchronization on an organization
- Don't display broken remote organization avatar if none is defined
- Return proper error code and message if an archive cannot be created before the request timeout ends
- Properly handle API rate limits in vcs synchronization worker

#### 1.3.12
*Jan 2, 2018*

**Features**
- Allow defining multiple mirrored Composer repositories with the same URL but different credentials

**Changes**
- Lower timeouts (2 seconds) on retrieving data from mirrored Composer repositories
- Clarify synchronization error messages, more specific and instructional
- Upgraded to PHP 7.2
- Upgraded Replicated to 2.14.0

#### 1.3.11
*Dec 13, 2017*

**Changes**
- Update UI structure for mirrored repositories in organization settings to match other settings
- Improved error messages in case Bitbucket API Limits are encountered
- Do not warn about SSL requirement for package URLs unless a user attempted to use a non-SSL http URL

**Bugfixes**
- Fix regression in form input for default member permission for synchronized GitHub organizations
- Display a helpful error message in case a Bitbucket organization cannot be loaded due to token issues instead of a 500 error on org settings

#### 1.3.10
*Dec 11, 2017*

**Changes**
- Increased timeouts on port listen startup events during docker orchestration

#### 1.3.9
*Dec 11, 2017*

**Bugfixes**
- Regression in GitHub Enterprise API client pagination

#### 1.3.8
*Dec 11, 2017*

**Bugfixes**
- Prevent 404 during Bitbucket privilege check in synchronization
- Correctly follow pagination URLs on GitHub Enterprise API in all cases (second fix)

#### 1.3.7
*Dec 8, 2017*

**Changes**
- Add new denormalized columns and indexes to improve performance of dependent/suggesters calculation for packages

**Bugfixes**
- Correctly follow pagination URLs on GitHub Enterprise API in all cases
- Don't include non-existing nginx log files in support bundle to prevent timeouts in support bundle generation

#### 1.3.6
*Dec 6, 2017*

**Features**
- Permanently display the most recent synchronization failure message and info on last successful run

**Changes**
- No requirement for 20GB on the root filesystem anymore, you should have at least 10GB mounted on /data and 10GB available to the system and replicated however
- Only administrators of Bitbucket Teams can setup synchronization with a Private Packagist organization

**Bugfixes**
- Increase nginx server_names_hash_bucket_size to 64
- Make sure temporary files are always cleaned up after creating an archive even in case of errors
- Catch some unexpected potential errors returned by Bitbucket during synchronization, log them and retry later

#### 1.3.5
*Nov 30, 2017*

**Features**
- Provide a manual option in org settings for default package access for all members on synchronized GitHub organizations because the GitHub API does not yet expose this setting to applications
- Synchronize packages in shared projects on GitLab into the Private Packagist organization like regular projects

**Bugfixes**
- If the owner of a GitLab sync auth token is not a member of the synchronized group, attempt to load all GitLab group info (only up to 20 API page requests)

#### 1.3.4
*Nov 28, 2017*

**Features**
- Errors and warnings in package update logs are now highlighted and indicated with a warning symbol
- For organizations synchronized with GitHub, create a team containing all members which are not in any team

**Changes**
- Allow deleting a credential currently in use by packages and mirrored repositories with an option to replace it with a different existing credential
- Retry synchronization jobs after a delay if they failed due to reaching a GitHub or Bitbucket rate limit
- Prevent the addition of non-SSL repository URLs upfront rather than erroring during update jobs later
- Logging in composer repository application is triggered at lower notice level now
- Don't retry gitlab API requests after 5 attempts that all timed out
- Schedule even github packages which have an oauth token for 12 hour updates in case the webhook isn't set up
- Added a view button for mirrored composer repositories on the organization settings page
- Improve debugging output in case the synchronized gitlab group cannot be found during synchronization
- Made database migration state detection more robust and removed error output on startup
- Updated Replicated to 2.13.1

**Bugfixes**
- Prevent race condition that could lead to broken zip files created for download from packages on Bitbucket or GitLab
- GitHub organization listing is no longer limited to 30 (API pagination)

#### 1.3.3
*Oct 13, 2017*

**Bugfixes**
- Fix regression introduced for archive creation in last release

#### 1.3.2
*Oct 13, 2017*

**Features**
- Allow users to regenerate their personal auth token on the profile page

**Changes**
- Improve guidance through Integration setup in Enterprise admin panel
- Enterprise Setup now checks for availability of ports 80 and 443
- PHP base image upgrade

**Bugfixes**
- Request correct scope on login with self-hosted GitLab CE OAauth
- Skip the account merge dialog if a user tries to login in a browser tab after having logged in on another one
- Prevent error when deleting users in Enterprise Setup mode without being logged in
- Fix race condition during archive generation that could lead to incorrect zip contents on download

#### 1.3.1
*Sep 25, 2017*

**Features**
- Option to delete organizations from the Enterprise admin panel without joining an organization as an owner
- Organization memberlist now shows all inactive users who will become members as soon as they activate their Private Packagist account by logging in

**Changes**
- Package update errors now differentiate between HTTP errors that may be caused by permission issues and errors caused by other networking problems, e.g. timeouts
- Postgres password is now read-only on the Enterprise Management Console as it was not supposed to be modified
- GitLab subgroup names are now display with the full hierarchy path when creating a new organization

**Bugfixes**
- Prevent race condition when a user tries to setup synchronization again before previous background job finished
- The dialog to create an organization from GitLab groups now lists all groups rathern than just the first 20
- Prevent error when deleting a user while logged out in Enterprise setup mode

#### 1.3.0
*Sep 21, 2017*

**Changes**
- Upgraded GitLab API client library, we now require GitLab API v4 - this means **GitLab versions lower than 9.0 are no longer compatible**

#### 1.2.7
*Sep 20, 2017*

**Bugfixes**
- Allow X-Forwarded-For headers on http requests to the Enterprise health check

#### 1.2.6
*Sep 19, 2017*

**Features**
- Add SMTP connection check button to Enterprise management console settings screen

**Bugfixes**
- Ensure users are no longer tied to an integration after it's been deleted
- Prevent synchronization from failing if teams get renamed to names of other now deleted teams

#### 1.2.5
*Sep 13, 2017*

**Features**
- Team package access page now lists synchronized packages without access to clarify which permissions are handled in external systems
- GitLab subgroups now have individual teams for different types of access (Developer, Reporter, Owner, ...)
- Bitbucket Server Integration Setup form now has detailed list of required settings for different dialogs

**Changes**
- Team renames are tracked on GitLab and GitHub, Bitbucket as no team identifier other than the name
- Increased timeouts for GitLab requests to 15 seconds to circumvent their rate limiting bugs

**Bugfixes**
- GitLab permissions are now correctly inherited to subroups if they are unchanged from the parent
- Prevent invalid composer.json on master branch from crashing package updates on other branches/tags

#### 1.2.4
*Sep 8, 2017*

**Bugfixes**
- Ensure internal random secret value is never modified on updates to prevent hook URLs from changing

#### 1.2.3
*Sep 7, 2017*

**Bugfixes**
- Fix URL added in previous release in specific cases

#### 1.2.2
*Sep 7, 2017*

**Bugfixes**
- Always show manual hook URL for packages where hook is not managed automatically

#### 1.2.1
*Sep 7, 2017*

**Bugfixes**
- Skip unnecessary query on executing workers, resulting in filling up postgres error log

#### 1.2.0
*Sep 5, 2017*

**New Features**
- Package rename handling: new package is automatically added if the package name changes in composer.json on the default branch of an existing package
- Team members can view which packages their own team has access to (previously for admins only)
- View full repository paths for GitLab subgroups
- Support for Typo3 Composer Repository and t3x archive files
- Shift-clicking update button on view package page force updates all versios

**Changes**
- Deleting a user in Enterprise Admin Panel requires confirmation
- Better error reporting in user interface during synchronized org creation failures
- Performance of team overview page improved for very large organizations
- Updated to latest version of Composer for improved GitLab compatibility (API v4) and full subgroup support
- Updated Replicated to 2.11.1
- Improved signal handling in worker processes
- Team page directly links to full member management page

**Bugfixes**
- Prevent deadlock in cache busting worker process
- Mount the composer working directory into the ui container to allow manual adding mirrored packages from the web interface
- Request OAuth Scopes on every GitLab request, fixes compatibility with new GitLab version and removes extra confirmation dialog on login
- User account merging can no longer result in error for users with audit log entries
- Deleting an integration properly disconnects synchronized organizations from the third party service
- Properly clear permission cache on package deletion (low impact, as package data was gone anyway)

#### 1.1.3
*Aug 4, 2017*

**New Features**
- Option to disable email entirely for Enterprise
- GitLab subgroup support

**Bugfixes**
- Handle GitHub Abuse responses appropriately
- Fix suggested GitLab authentication URL
- Do not attempt to mirror a package name that also exists publically but current user has no access to
- Avoid duplicate initialization of packages

#### 1.1.2
*Jul 25, 2017*

**Changes**
- Upgraded Replicated to 1.9.3

#### 1.1.1
*Jul 24, 2017*

**New Features**
- Ability to search and delete user accounts completely on admin page
- Display github rate limit issue warnings on package initialization
- Support for nested Bitbucket and GitLab repository URLs

**Changes**
- Higher timeouts and better handling of timeouts for Bitbucket and GitLab
- Warn users when trying to use readonly tokens to mirror new packages
- More reliable error logging on the repo container
- Unified job queue with priorities replaces workers separated by job type
- Allow organization admins to manage synchronization, not just owners
- Avoid probing for composer.json on VcsRepos that are known to work already
- Updated GitHub links to use new apps path (formerly integrations)

**Bugfixes**
- track state of packages correctly after sync has been disabled on an organization
- No more 403 page after removing yourself from an organization
- Prevent duplication of synchronized repositories in add package screen
- Package renames now invalidate cache of of composer metadata
- Unreachable third party mirrored repositories now trigger warning instead of 500 error on composer repo
- No 403 erros for non-admins when viewing package details of a package in their team

#### 1.1.0
*Jun 15, 2017*

**New Features**
- Allow editing of authentication token descriptions

**Changes**
- Remap all internally used ports to uncommon numbers to avoid conflicts with other services on host
- Report Webhook error states on package details page
- Use internal network only for internal API calls to avoid reliance on DNS or public certs
- Switched all containers to alpine to reduce size and improve compatibility
- Added more detailed logging to external API interaction in case of errors
- More detailed logging of all error states in docker process stdout / support bundle

**Bugfixes**
- Removed potential race condition in database setup for Private Packagist Enterprise
- Synchronize team memberships when manually adding a package in the synchronized organization
- Validate from mail address in dashboard settings
- Ensure hostname for ui and repo are distinct in dashboard settings
- other minor changes and dependency updates

#### 1.0.15
*May 18, 2017*

**Bugfixes**
- Fix regressions in background workers
- Fix python patch for supervisord

#### 1.0.14
*May 17, 2017*

**Bugfixes**
- Patching Python for RHEL 7.2 compatibility to run supervisord

#### 1.0.13
*May 17, 2017*

**Changes**
- System Updates
**Bugfixes**
- RHEL supervisord/python random syscall workaround

#### 1.0.12
*May 8, 2017*

**New Features**
- Store user join/leave activity on organizations

**Bugfixes**
- Organizations will not be created with a number suffix in their name unless necessary
- Fix deletion of organizations which resulted in a 500 error
- Per-organization seat-based notification emails are disabled
- Improve Bitbucket token issue error handling
- Ensure admin teams definitely always have access to all synchronized packages

#### 1.0.11
*Apr 23, 2017*

**Bugfixes**
- Fix GitHub Enterprise automatic webhook setup (differs from github.com)
- Automatically correct webhook setup on manual package update if not present or incorrect

#### 1.0.10
*Apr 20, 2017*

**New Features**
- New Organization Explorer in the Admin Panel
- Allow Deletion of Integrations
- Collect supervisord process log files in support bundle

**Bugfixes**
- Fix an organization synchronization error
- Allow all images in Content Security Policy to allow for external avatars
- Improve display of Installation Counts

#### 1.0.9
*Apr 19, 2017*

**New Features**
- HTTP Port is now configurable to something other than 80

#### 1.0.8
*Apr 19, 2017*

**Bugfixes**
- Fix regression in GitHub Enterprise API Access
- Improve Startup Time of nginx container

#### 1.0.7
*Apr 19, 2017*

**New Features**
- Added HTTPS Port Selection to allow custom HTTPS Ports

#### 1.0.0
*Mar 30, 2017*

**First stable release**
