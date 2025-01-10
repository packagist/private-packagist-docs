# Getting started with Conductor
##

Conductor will group and schedule automated Dependency Updates on your own Continuous Integration platform. If the update succeeds, Conductor sends you a pull request to your code hosting platform (GitHub, GitLab, Bitbucket) with the changed composer.lock file and, if necessary, other files modified by Composer plugins or scripts.

To use Conductor:

- You need to be approved for early access to Conductor. [Join to the waitlist](/features/conductor) and wait for approval.
- You need a Private Packagist trial or subscription on the cloud plan.
- You need to add a workflow to your Continuous Integration platform to run Composer updates, described below.

## Add packages to update automatically

Once you have a Private Packagist organization and Conductor is enabled for you, log into your Private Packagist organization and click on the "Updates" tab in the main navigation.  

Conductor will manage dependencies for packages added to Private Packagist [via synchronization](/features/integration-github-bitbucket-gitlab.md) that have a composer.lock file committed to the repository.

If you do not see your package, follow the instructions on your organization page to add the custom repository to the composer.json of your package.  

## Create a workflow on your CI

### GitHub Actions

Create a new GitHub Actions workflow in `.github/workflows/dependency-update.yaml` of your GitHub repository using the template below:

CONDUCTOR_GITHUB_ACTIONS_WORKFLOW

1. Adjust the PHP Version used in the "Install PHP" step
2. Commit and push the workflow to your main branch of your package repository

Create a secret `COMPOSER_AUTH` with the Composer authentication configuration [as described here](https://getcomposer.org/doc/articles/authentication-for-private-packages.md#authentication-using-the-composer-auth-environment-variable) to access Private Packagist.  
We recommend to create a dedicated authentication token with update access. You can copy and paste the contents for the secret from the Private Packagist UI while creating the token in "Settings" -> "Authentication Tokens". Remove the single quotes around the value.

![Create Authentication Token](/Resources/public/img/docs/conductor/authentication-token.png)

The contents of the variable should look like 

```json
{"http-basic": {"repo.packagist.com": {"username": "token", "password": "packagist_out_73a81c..." }}}
```

Conductor needs to [verify your CI setup](#verify-your-ci-setup) before you can start receiving pull requests.

## How scheduling works

- Navigate to the "Updates" tab in your Private Packagist organization 
- Click on the name of your package

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

If all these steps succeeded, Private Packagist creates a pull request for the just pushed branch. The PR description will contain details about the update and changelogs from your dependencies. Conductor integrates with [Update Review](https://packagist.com/features/update-review) to present a reviewable list of all updated dependencies.

![Conductor Pull Request](https://packagist.com/img/features/auto-updates/merged-PR-for-a-security-updated.png)

Once you reviewed the changes and merged the PR, Conductor will schedule the next task.      
If you close the PR, the task will be paused and Conductor will schedule the next task. This is the same effect as using the "Pause" button in the UI. Conductor won't attempt to update the dependency to this exact version again but will schedule updates with newer versions.  

If you want to schedule any other task in the list, click on its name and use the button "Schedule now to create a PR".

Tasks fixing security issues have a higher priority. They will be moved to the top of the list and scheduled right away even if there already is a PR for another task open.

## Verify your CI setup

![Task list with verification task](/Resources/public/img/docs/conductor/verification-task-list.png)

Right now all tasks are waiting for the CI verification task on top of the list. Conductor will not start with the regular schedule until this verification task was successful.
The verification task will only execute `composer update nothing` and will not result in a PR to be sent to your code hosting platform.  

- Click on the task "Verify the continuous integration setup"
- Use the "Schedule now" button to test your setup

You can see the state of your task and the last events for the task. Once the task is executed, watch your CI platform:
You should see a run for the just added workflow. Examine the run if it succeeded.

If it was successful your CI configuration is verified and complete. Conductor will trigger your workflow with the next task in the list. This time it will send a pull request. 

When you run into errors, troubleshoot and fix them. You can trigger the workflow again by restarting the CI verification task. The restart button is available after the first execution.  
