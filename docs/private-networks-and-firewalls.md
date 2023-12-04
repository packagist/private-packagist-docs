# Private Networks and Firewalls
## Using Private Packagist to access code in your internal company network

Private Packagist requires access to your source code for several reasons:

1. Private Packagist needs to list all tags and branches on your version control system repositories to identify the available versions which Composer can install.
1. Private Packagist needs to collect the metadata from composer.json files stored in your version control system to make the information available to Composer commands.
1. Private packagist needs to export zip files of specific revisions of your packages' source code for fast installation through Composer without cloning or checking out your entire source code repository on CI or deployment systems.
1. If you wish to use an integration with your code hosting solution (e.g. GitHub Enterprise, GitLab Self-Managed, Bitbucket Server) Private Packagist will need access to their API to allow logins with OAuth and to provide listings of teams and repositories.
1. Features like Update Review which comment on your pull requests or create pull requests need to access your code hosting solution via API and VCS to do so.

If the source code for your private packages is stored in your company's private network behind a firewall and you would like to use Private Packagist you have two options:

1. Use our on-premises product [Private Packagist Self-Hosted](https://packagist.com/self-hosted) which you can host inside your company's private network with full access to your source code.
1. Use our SaaS solution Private Packagist Cloud and ensure your version control system can be reached by our servers as explained below.

## Granting Private Packagist Cloud Network Access

### Public IP and DNS
You will be adding packages to Private Packagist using a URL to the respective version control repository. You need to ensure that you either have public DNS records pointing to the machines on your internal network or you need to directly use the respective IP addresses. If your version control system server does not have a public IP, Private Packagist cannot connect to it.

### Firewall Configuration
You need to allow connections from Private Packagist Cloud servers to servers behind your firewall. We recommend that you accept external connections from all IPs in the document at <https://packagist-network.s3.eu-west-1.amazonaws.com/ip-address-list>. This list is updated automatically to contain a current list of our IP addresses used to access your source code or your code hosting solution. While it does not change frequently, we may modify it at any time. For this reason, we recommend that you use your firewall's mechanism to reference an external list of IPs with this URL rather than manually copying the IPs which will require frequent maintenance on your end. Additionally the DNS name `outgoing.packagist.com` resolves to a full list of all these IPv4 and IPv6 addresses as well, so if your firewall can use a DNS name instead to grant us access, you may use this record.
