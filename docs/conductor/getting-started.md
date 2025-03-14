# Getting started with Conductor
##

Conductor will group and schedule automated dependency updates on your own continuous integration platform. If the update succeeds, Conductor will send you a pull request to your code hosting platform (GitHub, GitLab, Bitbucket) with the changed composer.lock file and, if necessary, other files modified by Composer plugins or scripts.

Prerequisites for receiving dependency update PRs from Conductor:

- Receive early access to Conductor. [Join the waitlist](/features/conductor) and wait for approval.
- Set up an organization on Private Packagist Cloud either with a free trial or subscribe to the cloud plan.
- A synchronization in your Private Packagist organization with your code hosting platform.
- A workflow on your continuous integration platform to run Composer updates.

## Set up synchronization

Once you have a Private Packagist organization and Conductor is enabled for you, log into your Private Packagist organization and head to the "Settings" tab in the main navigation.
Under the "Synchronization" entry you can add [one or more synchronizations](/features/integration-github-bitbucket-gitlab.md) with an organization on your code hosting platform.
This is how you grant us access to your VCS repositories. The synchronization will automatically add any repository with a composer.json file in the root directory as a package to Private Packagist.

## Configure Conductor for your packages

Access the "Conductor" tab in the main navigation to see a list of available packages that can have their dependencies updated by Conductor.
Each package shown is linked to its VCS repository to which Conductor will send pull requests with dependency updates. Your Composer projects or applications are also a kind of package and must be added to Private Packagist as regular packages to use Conductor on them.
Conductor can only manage dependencies for packages added to Private Packagist [via synchronization](/features/integration-github-bitbucket-gitlab.md) that have a composer.lock file committed to the repository.

To get started, click on the configure link displayed next to the package which you would like Conductor to update. Follow the instructions for your continuous integration platform.

### GitHub Actions

Create a new GitHub Actions workflow in `.github/workflows/conductor.yaml` of your GitHub repository to use the 
[Conductor GitHub Action](https://github.com/packagist/conductor-github-action) with the template below:

CONDUCTOR_GITHUB_ACTIONS_WORKFLOW

1. Adjust the PHP Version used in the "Install PHP" step
2. Commit and push the workflow to your main branch of your GitHub repository

If your package requires access to your Private Packagist Composer repository then Conductor will automatically create
[short-lived authentication tokens](../composer-authentication.md#conductor-authentication-tokens) to run Composer commands in your CI environment.
Similar to organization authentication tokens, access can be restricted to any set of packages to which any of the organization's teams has access.

Once the workflow is added to your GitHub repository, Conductor needs to verify your CI setup before you can start receiving PRs.

### Skip Composer scripts

If you've configured Composer to run scripts for certain events that make changes which are irrelevant for the Conductor PR, you can skip 
these script handlers by setting the [COMPOSER_SKIP_SCRIPTS](https://getcomposer.org/doc/03-cli.md#composer-skip-scripts) environment variable.

For example, if you use `npm` to build static assets in your `post-install-cmd` script you can update your GitHub action to skip it:

```yaml
- name: "Running Conductor"
  uses: packagist/conductor-github-action@v1
  env:
    COMPOSER_SKIP_SCRIPTS: "post-install-cmd"
```

Please note that this feature is only available since [Composer 2.8.6](https://github.com/composer/composer/releases/tag/2.8.6), so make sure your CI 
installs the latest version.

## Verify your CI setup

- Navigate to the "Conductor" tab in your Private Packagist organization.
- Click on the name of your package.

![Task list with verification task](/Resources/public/img/docs/conductor/verification-task-list.png)

Right now all tasks are waiting for the CI verification task on top of the list. Conductor will not start with the regular schedule until this verification task was successful.
The verification task will only execute `composer update nothing` and will not result in a PR to be sent to your code hosting platform.

- Click on the task "Verify the continuous integration setup"
- Use the "Schedule now" button to test your setup

You can see the state of your task and the last events for the task. Once the task is executed, watch your CI platform:
You should see a run for the just added workflow. Examine the run to see if it succeeded.

If it was successful your CI configuration is verified and complete. Conductor will trigger your workflow with the next task in the list. This time it will send a pull request.

When you run into errors, troubleshoot and fix them. You can trigger the workflow again by restarting the CI verification task. The restart button is available after the first execution.

## How scheduling works

The list shows groups of all available updates to be scheduled. Each group of updates is called a task. Conductor will schedule only one task at a time. All others are waiting for the task on top of the list to be successful or paused.  
Once Conductor schedules a task it sends a payload to your CI platform that triggers the workflow you just added. The payload contains the commands Composer will run to update a group of dependencies from your package.

The workflow consists of several steps:

1. Checkout the code from your repository
2. Set up PHP
3. Run `composer install`
4. Run Composer update commands
5. Commit changed files (composer.lock, ...)
6. Push commits to a new branch (or force push an existing branch)
7. Send the status of the workflow to Private Packagist

If all these steps succeeded, Private Packagist creates a pull request for the newly pushed branch. The PR description will contain details about the update and changelogs from your dependencies. Conductor integrates with [Update Review](https://packagist.com/features/update-review) to present a reviewable list of all updated dependencies.

![Conductor Pull Request](https://packagist.com/img/features/auto-updates/merged-PR-for-a-security-updated.png)

Once you reviewed the changes and merged the PR, Conductor will schedule the next task.      
If you close the PR, the task will be paused and Conductor will schedule the next task. Clicking the "Pause" button in the UI has the same effect. Conductor won't attempt to update the dependency to this exact version again but it will schedule updates to newer versions.

If you want to schedule any other task in the list, click on its name and use the button "Schedule now to create a PR".

Tasks fixing security issues have a higher priority. They will be moved to the top of the list and scheduled right away even if there already is a PR for another task open.

