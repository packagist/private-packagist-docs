# Pull Request Comments

Private Packagist pull request comments help you review dependency changes in your VCS repositories. Every time someone
changes a composer.lock file in one of your repositories, we comment on the pull request with a readable summary, links
to diffs and changelogs for you to review. Pull request comments are available for synchronizations with
GitHub (Enterprise), GitLab and Bitbucket.

We detect two kinds of changes:
* Packages that are added, removed or where the version changes
* Packages with important metadata changes, like changes to the source or dist URL for instance when you are switching from or to a fork

If you installed our GitHub App, then the private-packagist bot will comment on your pull requests. In all other cases, 
the comments will be created by the credential owner. Please note that for synchronizations with Bitbucket, you will 
have to grant us access to the `pullrequest:write` scope, if you haven’t already, to benefit from the feature.

### Configuring pull request comments
Pull request comments are enabled by default for all supported synchronizations, and you can configure settings for each
synchronization individually. 

We’ll keep editing the comment to make sure it always reflects the latest state of the composer.lock files. By default,
we add a one-line comment with a link to the original comment to the pull request every time we edit to make sure you
didn’t miss any changes.

If you are on GitHub and use Dependabot, then you also have the option to disable our comments for pull requests created
by Dependabot. However, we recommend keeping them on because we noticed that Dependabot will sometimes perform major
updates to dependencies without mentioning them in the pull request description. You'd only notice them by manually
reviewing the JSON lock file.

![Pull request comment example on GitHub](/Resources/public/img/docs/features/PullRequestComment-20211125.png)
