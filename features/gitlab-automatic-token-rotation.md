# Automatic GitLab Token Rotation
## 

Your GitLab access token expired again, breaking your `composer install` in production at 3 AM. Sound familiar?

This became a widespread problem starting in May 2023 when GitLab began requiring all new tokens to expire within 365 days, then reached a crisis point in May 2024 when existing non-expiring tokens were forced to expire on May 14, 2024. Many developers were caught off guard when their long-running integrations suddenly stopped working.

Private Packagist automatically rotates your GitLab tokens before they expire, eliminating those surprise failures that disrupt your development workflow and break CI/CD pipelines.

## How automatic token rotation works

Private Packagist monitors your GitLab tokens and automatically rotates them 24 hours before expiration. No more emergency scrambles to update tokens across your development team, CI systems, and production environments.

When a token is about to expire, Private Packagist uses GitLab's official rotation API to seamlessly create a new token and update your credentials in the background. Your existing composer projects, CI/CD systems, and development workflows continue without any interruptions.

### Requirements for automatic rotation

Your GitLab token must have these scopes:
- `api` - Required for GitLab API access
- `read_user` - Required to read user information  
- `self_rotate` - Required for automatic token rotation

<div class="row column">
    <div class="callout warning">
        <p>Tokens without the <code>self_rotate</code> scope cannot be automatically rotated and may cause service interruptions when they expire.</p>
    </div>
</div>

## Setting up automatic token rotation

### Creating a GitLab token with rotation support

1. Go to your GitLab instance (GitLab.com or self-hosted)
2. Navigate to token settings:
   - **Personal tokens**: Profile → Access Tokens
   - **Group tokens**: Group Settings → Access Tokens  
   - **Project tokens**: Project Settings → Access Tokens
3. Create a new token:
   - Enter a descriptive name like "Private Packagist"
   - Set your preferred expiration date
   - Check the scopes: `api`, `read_user`, and `self_rotate`
4. Generate the token and copy it immediately
5. Add the token to Private Packagist in your credentials section

### GitLab version requirements

| GitLab Version | Automatic Rotation |
|---------------|-------------------|
| 17.9.0 and later | ✅ Supported |
| Before 17.9.0 | ❌ Not supported |
| GitLab.com | ✅ Always supported |

## Works with all token types

Whether you use personal access tokens, group access tokens, or project access tokens, Private Packagist handles the rotation seamlessly:

**Personal Access Tokens**  
Created by individual users with their account permissions.

**Group Access Tokens**  
Created at the group level, recommended for organizational use.

**Project Access Tokens**  
Scoped to specific projects with project-level permissions.

## Monitoring your tokens

Check your credentials section in Private Packagist to view:
- Token expiration dates
- Available scopes
- Last rotation date

Rotation activities are logged in your organization log for audit and security purposes.

## Troubleshooting rotation issues

### Token not rotating automatically
- Verify your token has the `self_rotate` scope
- Check your GitLab version is 17.9.0 or later
- Ensure the token hasn't already expired

### GitLab version too old
- Upgrade to GitLab 17.9.0 or later for automatic rotation
- Manually renew tokens for older GitLab versions

**Ready to eliminate token expiration headaches?** Private Packagist's automatic GitLab token rotation runs in the background, ensuring your development workflow doesn't get interrupted by expired credentials again.