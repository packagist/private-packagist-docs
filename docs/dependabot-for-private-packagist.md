# Set up dependabot with Private Packagist
## 

[Dependabot](https://dependabot.com) is a security feature from GitHub, that analyses security issues in one of your dependencies and may create Pull Requests to update those dependencies automatically.

Dependabot can update your composer.lock file in Pull Request and you can set it up to analyse packages on packagist.com. This guide explains how to do this step by step.

## Enable Dependabot in GitHub

Start in your GitHub repository and go to “Settings”. In the “Security” section of the sidebar go to “Code security and analysis”, where you can enable Dependabot. In the process you will create a dependabot.yaml that should look like below. [Refer to this guide on GitHub, for the complete steps](https://docs.github.com/en/code-security/getting-started/dependabot-quickstart-guide#enabling-dependabot-for-your-repository)

![enable dependabot](/Resources/public/img/docs/articles/dependabot-secret.png)

When you click on configure, the dependabot.yaml will be created for you (it will be placed in a folder .github).

## dependabot.yaml

A minimal dependabot config file would look like this:

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

Replace the Composer url `https://repo.packagist.com/acme/` with your own organizations Composer url on packagist.com. In the example the repository url is for the organization on Private Packagist Cloud with the name `acme`.  

To allow dependabot to analyse the packages on Private Packagist, we need to provide the secret `PRIVATE_PACKAGIST_PASSWORD` in your GitHub repository. In Settings under the section Security there is  Secrets and Variables for Dependabot.

![Dependabot Secrets](/Resources/public/img/docs/articles/dependabot-secret.png)

Create a Team Authentication Token under “Settings” and “Authentication Tokens” on Private Packagist. Copy the secret into the GitHub form.

## Troubleshooting

To check if Dependabot is able to access your packages, you need to navigate to “Insights”, then “Dependency Graph” and then activate the tab “Dependabot”. 

![Dependabot Insights](/Resources/public/img/docs/articles/dependabot-insights.png)

There is a link “Recent update jobs”, that will show the last jobs from dependabot and some logs. The last job should be green now.

![Dependabot Jobs](/Resources/public/img/docs/articles/dependabot-jobs.png)

- Make sure, that the secret name, matches the one that you put to the dependabot.yaml
- Check if the token is still valid and has not expired.
- the names under `updates[x].registries` must match a key for credentials in `registries` on the toplevel of the dependabot.yaml
