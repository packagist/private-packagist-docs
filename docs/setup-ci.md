# Using Private Packagist in different CI/CD environment


## Bitbucket Pipeline

On Bitbucket Pipelines you need to setup an environment variable called `COMPOSER_AUTH` which you can find on your on your Organization’s overview page under environment variable.
Read-only token are found on your organization’s settings page under *Authentication tokens*:
```
COMPOSER_AUTH='{"http-basic": {"repo.packagist.com": {"username": "token", "password": "TOKEN_HERE"}}}'
```
You can add the `COMPOSER_AUTH` environment variables at the workspace, repository, and deployment levels on Bitbucket Pipelines as mentioned in [their documentation](https://support.atlassian.com/bitbucket-cloud/docs/variables-and-secrets/).
