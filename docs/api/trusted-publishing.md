# Trusted Publishing for artifact packages
##

Trusted publishing uses [OpenID Connect (OIDC)](https://openid.net/connect/) to publish artifact packages directly from your
CI/CD workflows without the need for long-lived API credentials. Trusted publishing is an [industry standard](https://repos.openssf.org/trusted-publishers-for-all-package-repositories) 
defined by the Open Source Security Foundation (OpenSSF) and implemented by various package registries that host package artifacts like PyPI and RubyGems. 

## How does it work?

OIDC identity providers (in this context CI services like GitHub Actions), can issue short-lived credentials (OIDC tokens), that Private Packagist can verify came from a trusted CI service run. Organizations on Private Packagist can configure to trust a workflow in a repository to publish a package. 
The workflow sends an OIDC token to Private Packagist, where the token is matched against configured trusted publishers.
If there is a match, Private Packagist will issue a short-lived API credential with limited scope. 
The issued API credential is valid for 15 minutes and can only access endpoints required to publish the artifact.

## Supported CI/CD providers

Private Packagist currently supports the following CI/CD providers:
* GitHub Actions

## Configure trusted publishing

Organization owners and admins can configure a trusted publisher on the API access settings page. A configured trusted publisher
will allow one package to be published from a single CI/CD workflow. If a repository publishes multiple packages or if a
package gets published from multiple workflows or repositories, then multiple publishers need to be configured.

If you previously used API credentials to interact with the Private Packagist API to upload artifacts, make sure you take
a look at the API credentials section on the same page to see if you have any credentials that are no longer needed.

### GitHub Actions

Fill in the form fields to configure the publisher:
* Package name: The name of the existing package or the package that will be created when this publisher is used.
* Owner name: The GitHub user or organization name that owns the repository.
* Repository name: The name of the GitHub repository that contains the publishing workflow.
* Continuous integration file: The filename of the publishing workflow, e.g. `publish.yaml`. The file must exist in the `.github/workflows/` directory.
* Continuous integration environment name (optional): The name of the [GitHub Actions environment](https://docs.github.com/en/actions/how-tos/deploy/configure-and-manage-deployments/manage-environments) that the workflow uses.

## Configure your CI/CD workflow

### GitHub Actions

Private Packagist provides a GitHub Action [packagist/artifact-publish-github-action](https://github.com/packagist/artifact-publish-github-action),
that takes care of publishing the artifact for you. Just build the artifact and hand its path over to the action. The action
requires the `id-token: write` permission to generate OIDC tokens, more info about this in [GitHub's OIDC documentation](https://docs.github.com/en/actions/concepts/security/openid-connect).

```yaml
name: Private Packagist Publish Artifact

on: # Define when to run this workflow, e.g. when a new tag is pushed
      
permissions:
  id-token: write # Required for OIDC
  contents: read  # Required to checkout the repository
  
jobs:
    publish_artifact:
        runs-on: "ubuntu-latest"

        steps:
            - uses: actions/checkout@v5
              
            # Create your artifact file here

            - name: "Publish artifact"
              uses: packagist/artifact-publish-github-action@v1
              with:
                package_name: 'acme/package'
                organization_url_name: 'acme-org'
                artifact: '/full/path/to/artifact.zip'
```

