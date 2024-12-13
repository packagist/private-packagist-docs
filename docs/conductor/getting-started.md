# Getting started with Conductor
##

Conductor will group and schedule automated Dependency Updates on your own Continuous Integration Platform. If the update succeeds, Conductor sends you a pull request to your code hosting platform (GitHub, GitLab, Bitbucket) with the changed composer.lock file and, if necessary, other files modified by Composer plugins or scripts.

To use Conductor:

- [subscribe to the waitlist](http://packagist.com.lo/features/conductor)
- subscribe to a Private Packagist Subscription (either Cloud or Self-Hosted)
- get access to Conductor from [Private Packagist Support](mailto:contact@packagist.com)
- add a workflow to your Continuous Integration Platform to run Composer updates
- verify the setup


## First steps

Once you have a Private Packagist Subscription and Conductor is enabled for your subscription, log into your Private Packagist organization and click on the Updates tab in the main navigation.  
Conductor will list all Packages with a composer.lock file and the Private Packagist repository added to the composer.json.    
If you do not see your package, follow the instructions on your organization page to add the custom repository to the composer.json of your package.  

Most convenient way to set up Conductor is to configure your CI in the Private Packagist UI. The manual steps are outlined below:

## Create a workflow on your CI

### GitHub Actions

Create a new GitHub Actions workflow in `.github/workflows/dependency-update.yaml` of your package repository using the template below:

CONDUCTOR_GITHUB_ACTIONS_WORKFLOW

- adjust the PHP Version used in the "Install PHP" step
- commit and push the workflow to your main branch of your package repository
- review the commit and push method: the workflow needs to be able to push commits to a new branch

Create a secret `COMPOSER_AUTH` with the Composer authentication configuration [as described here](https://getcomposer.org/doc/articles/authentication-for-private-packages.md#authentication-using-the-composer-auth-environment-variable) to access Private Packagist.  
We recommend to create a dedicated authentication token with update access. You can copy n paste the contents for the secret from the Private Packagist UI while creating the token in "Settings" -> "Authentication Tokens". Remove the single quotes around the value.

![Create Authentication Token](/Resources/public/img/docs/conductor/authentication-token.png)

The contents of the variable should look like 

```json
{"http-basic": {"repo.packagist.com": {"username": "token", "password": "packagist_out_73a81c7eb525b13b6bc22a410b2146e78a38b324f609bf1158c583c704f64cbdd349" }}}
```

Conductor needs to verify your setup before you can [start receiving Pull Requests](#how-scheduling-works). 

## How scheduling works

- Go to your package on the updates tab in your Private Packagist organization 
- Click on the name of your package

The list shows groups of all available updates to be scheduled. Each group of updates is called a task. Conductor will schedule only one task at a time. All others are waiting for the task on top of the list to be successful or paused.  
Once Conductor schedules a task it sends a payload to your CI Platform that triggers the workflow you just added. The payload contains the commands Composer will run to update a group of dependencies from your package.

The workflow consists of several steps:

- code checkout
- php setup
- Composer install
- run Composer update commands from the payload
- commit changed files (composer.lock, ...)
- push commit to a new branch
- Send the status of the workflow to Private Packagist via curl

If the update succeeded, Private Packagist creates a pull request for the just pushed commit. The PR description will contain a reviewable list of all dependencies and parts of their changelogs.  
When you reviewed the changes and merged the PR, Conductor schedules the next task.    
If you close the PR, the task will be paused and Conductor schedules the next task. This is the same effect as using the "Pause" button in the UI.  
If you want to, you can schedule any other task of the list, by clicking on it's name and using the button "Schedule now to create a PR".  

Tasks fixing security issues have a higher priority. They will be moved to the top of the list and scheduled right away.  

## Verify your CI setup

![Task list with verification task](/Resources/public/img/docs/conductor/verification-task-list.png)

Right now all tasks are waiting for the CI verification task on top of the list. Conductor will not start with the regular schedule until this verification task was successful.
The verification task will only execute `composer update nothing` and will not result in a PR to be sent to your Code Hosting Platform.  

- Click on the task "Verify the continuous integration setup"
- Use the "Schedule now" button to test your setup

You can see the state of your task and last events for the task. Once the task is executed, atch your CI Platform: 
You should see a run for the just added workflow. Examine the run if it succeeded.

When you ran into errors, troubleshoot and fix. You can trigger the workflow again by restarting the task. The restart button is available after the first execution.

Once the task is successful Conductor will trigger your workflow with the next task in the list. This time it will send a PR.
