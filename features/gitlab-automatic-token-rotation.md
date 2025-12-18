# Automatic GitLab Token Rotation
## 

Your GitLab access token expired again, breaking your `composer install` in production at 3 AM. Sound familiar?

This became a widespread problem starting in May 2023 when GitLab began requiring all new tokens to expire within 365 days, then reached a crisis point in May 2024 when existing non-expiring tokens were forced to expire on May 14, 2024. Many developers were caught off guard when their long-running integrations suddenly stopped working.

Private Packagist automatically rotates your GitLab tokens before they expire, eliminating those surprise failures that disrupt your development workflow and break CI/CD pipelines.

## How automatic token rotation works

Private Packagist monitors your GitLab tokens and automatically rotates them 24 hours before expiration. No more emergency scrambles to update tokens across your development team, CI systems, and production environments.

When a token is about to expire, Private Packagist uses GitLab's official rotation API to seamlessly create a new token and update your credentials in the background. Your existing composer projects, CI/CD systems, and development workflows continue without any interruptions.

### Requirements for automatic rotation

Your GitLab token must have either the `api` scope (which includes rotation capabilities) or the `self_rotate` scope for rotation-only permission.

<div class="row column">
    <div class="callout warning">
        <p>Most GitLab integrations already use the <code>api</code> scope, which supports automatic rotation. Tokens without either <code>api</code> or <code>self_rotate</code> scope cannot be automatically rotated.</p>
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
   - Check the required scopes: `api` and `read_user` (the `api` scope enables automatic rotation)
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

Private Packagist displays a warning icon with expiration information for credentials that are expiring soon or have already expired.

## Troubleshooting rotation issues

### Token not rotating automatically
- Verify your token has the `self_rotate` scope
- Check your GitLab version is 17.9.0 or later
- Ensure the token hasn't already expired

### GitLab version too old
- Upgrade to GitLab 17.9.0 or later for automatic rotation
- Manually renew tokens for older GitLab versions

**Ready to eliminate token expiration headaches?** Private Packagist's automatic GitLab token rotation runs in the background, ensuring your development workflow doesn't get interrupted by expired credentials again.