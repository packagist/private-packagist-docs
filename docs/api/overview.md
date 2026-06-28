# API documentation
## 

Private Packagist offers a REST API to manage your organization programmatically, as well as endpoints to read package metadata. This section explains how to authenticate, work with paginated responses, and publish packages from your CI/CD workflows.

- [REST API](/docs/api): Interactive reference for all available REST API endpoints.
- [REST API authentication](./api-authentication.md): How to authenticate your API requests, including the PHP client library and request signing with your token and secret.
- [REST API pagination](./api-pagination.md): How list endpoints split results into pages and how to retrieve large result sets.
- [Package Metadata API](./package-metadata.md): How to read package metadata programmatically through the Composer API on your repository.
- [Trusted Publishing](./trusted-publishing.md): Publish artifact packages from your CI/CD workflows using OpenID Connect (OIDC) instead of long-lived API credentials.
