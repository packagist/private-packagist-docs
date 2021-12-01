# Update Review

Private Packagist Update Review creates comments on your pull requests to help you review dependency changes in your VCS
repositories. Every time a composer.lock file changes in one of your repositories, we comment on the pull request with a
human-readable summary, links to diffs and changelogs for you to review. Update Review comments are available for synchronizations
with GitHub, GitLab, and Bitbucket as well as GitHub Enterprise, and self-managed GitLab.

We detect two kinds of changes:
* Package changes: Additions, removals, upgrades, and downgrades
* Important metadata changes: Changes to the source or dist download URLs or if the commit reference changes for an otherwise unmodified version number

For a better overview, packages required directly in your composer.json are highlighted in bold. Other packages in regular font are installed as dependencies of your own composer.json dependencies.

If you installed our GitHub App, then the private-packagist bot will comment on your pull requests. In all other cases, 
the comments will be created by the credential owner. Please note that for synchronizations with Bitbucket, you will 
have to grant us access to the `pullrequest:write` scope, if you haven’t already, to benefit from the feature.

### Configuring Update Review
Update Review is enabled by default for all supported synchronizations, and you can configure settings for each
synchronization individually. 

We’ll keep editing the comment to make sure it always reflects the latest state of the composer.lock files. By default,
we add a one-line comment with a link to the original comment to the pull request every time we edit it to make sure you
don’t miss any changes.

If you are on GitHub and use Dependabot, then you also have the option to disable our comments for pull requests created
by Dependabot. However, we recommend keeping them on because we noticed that Dependabot will sometimes perform major
updates to dependencies without mentioning them in the pull request description. You'd only notice them by manually
reviewing the JSON lock file.

![Update Review example on GitHub](/Resources/public/img/docs/features/UpdateReview-20211125.png)
