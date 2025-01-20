# Private Composer Packages
##

Private Packagist makes the code in your private repositories available for use with Composer.

## Where can Private Packagist get code from?
Private Packagist can access private code in any Git, Mercurial or Subversion repository with SSH or HTTP Basic authentication. This includes code stored on any of the following systems:
* GitHub
* GitHub Enterprise
* GitLab.com
* GitLab Self-Managed
* Bitbucket.org / Cloud
* Bitbucket Data Center / Server

## How does the setup in Private Packagist work?
First configure a set of credentials for the service you are using to store private code. Then add any number of packages by entering the respective repository URLs. Private Packagist will download your code, inspect all composer.json metadata and make the packages with all their versions available for browsing in your account.

If you are using our GitHub synchronization we will list all your repositories automatically, and you can pick which ones to add with a single click.

## How do I use Private Packagist in Composer?
Add the Private Packagist repository to your composer.json and require the packages by name as you are used to. If you already had private version control system repositories configured, you can remove them to speed up composer update.

<pre>
<code>
{
    "repositories": [
        <span class="strikethrough">{"type": "vcs", "url": "https://github.com/<i>your-org-name</i>/foo"},</span>
        <span class="strikethrough">{"type": "git", "url": "https://github.com/<i>your-org-name</i>/bar.git"},</span>
        {"type": "composer", "url": "https://repo.packagist.com/<i>your-org-name</i>/"},
        {"packagist.org": false}
    ],
    "require": {
        "org/foo": "^1.2.3",
        "org/bar": "dev-master"
    }
}
</code>
</pre>
**Note:** Your composer.lock file contains the download URLs for packages. You need to run composer update at least once to regenerate your composer.lock file before composer install will download files from Private Packagist.

## How do I configure a VCS repository with multiple composer.json files?

Private Packagist supports multiple packages per VCS repository, but you'll need to specify where the `composer.json` files are located.

When adding a new package, select the option _Repository contains multiple packages or composer.json is located in a subdirectory_. You can either specify the path to the `composer.json` file directly (if you have a single package in a subdirectory), or use a glob pattern such as `{lib,src}/**/composer.json` to specify multiple locations.

### Globbing

- `?` matches any character
- `*` matches zero or more characters, except `/`
- `/**/` matches zero or more directory names
- `[abc]` matches a single character `a`, `b` or `c`
- `[a-c]` matches a single character `a`, `b` or `c`
- `[^abc]` matches any character but `a`, `b` or `c`
- `[^a-c]` matches any character but `a`, `b` or `c`
- `{ab,cd}` matches `ab` or `cd`

A glob pattern of `**/**` will find all `composer.json` files in the repository.
