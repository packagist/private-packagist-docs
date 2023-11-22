# Set up Dependabot with Private Packagist
##

[Dependabot](https://docs.github.com/en/code-security/getting-started/dependabot-quickstart-guide) informs you about vulnerabilities in the dependencies that you use in your repository and can automatically raise pull requests to keep your dependencies up-to-date.  

This guide explains how to configure and set up Dependabot if you want to use it with [Private Packagist](https://packagist.com).


## Enable Dependabot in GitHub

Start in your GitHub repository and go to “Settings”. In the “Security” section of the sidebar go to “Code security and analysis”, where you can enable Dependabot. In the process, you will create a dependabot.yaml [as shown as below](#dependabotyaml). Follow [this guide on GitHub](https://docs.github.com/en/code-security/getting-started/dependabot-quickstart-guide#enabling-dependabot-for-your-repository) for the complete steps to enable Dependabot.

![enable dependabot](/Resources/public/img/docs/articles/dependabot-secret.png)

When you click on configure, the dependabot.yaml will be created for you in the .github folder.

### dependabot.yaml

A minimal Dependabot config file would look like this:

```yaml
version: 2
updates:
  - package-ecosystem: "composer"
    directory: "/"
    registries:
      - private_packagist
    schedule:
      interval: "weekly"
registries:
  private_packagist:
    type: composer-repository
    url: https://repo.packagist.com/acme/
    username: token
    password: ${{secrets.PRIVATE_PACKAGIST_PASSWORD}}
```

Replace the Composer URL `https://repo.packagist.com/acme/` with your organization's Composer URL on packagist.com. In the example, the repository URL is for the organization on Private Packagist Cloud with the name `acme`.

To grant Dependabot access to the packages on Private Packagist, you need to provide the secret `PRIVATE_PACKAGIST_PASSWORD` in your GitHub repository. In Settings, under the section “Security,” there is “Secrets and Variables” for Dependabot.

![Dependabot Secrets](/Resources/public/img/docs/articles/dependabot-secret.png)

Create a Team Authentication Token under “Settings” and “Authentication Tokens” on Private Packagist with update access. Copy the secret into the GitHub form.

## Troubleshooting

To check if Dependabot is able to access your packages, navigate to “Insights”, then “Dependency Graph” and then activate the tab “Dependabot”.

![Dependabot Insights](/Resources/public/img/docs/articles/dependabot-insights.png)

There is a link “Recent update jobs”, that will show the last jobs from Dependabot and logs. The last job should be green now.

![Dependabot Jobs](/Resources/public/img/docs/articles/dependabot-jobs.png)

Make sure that:
- the secret name matches the one that you referenced in dependabot.yaml
- the Private Packagist authentication token is still valid and has not expired.
- the names under `updates[x].registries` are matching a key for credentials in `registries` on the top level of the [dependabot.yaml](#dependabotyaml).
