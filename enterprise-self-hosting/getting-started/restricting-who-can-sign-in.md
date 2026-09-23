---
description: >-
  Restrict which usernames can sign in to your self-hosted Cobrowse instance.
---

# Restricting who can sign in

The `allowed_usernames` config value is a regular expression that every username must match before a user can sign in to, or be created on, your instance. It applies to every sign-in and provisioning method.

The username that is matched depends on the authentication method:

| Authentication method                                 | Username matched                   |
| ----------------------------------------------------- | ---------------------------------- |
| Magic link, Google, GitHub, Intercom, Zendesk          | The email address                  |
| [JWT](../../agent-side-integrations/json-web-tokens-jwts/) | The `sub` claim                    |
| [SAML 2.0](../../agent-side-integrations/authentication-saml-2.0.md) | The `NameID`                       |
| [SCIM](../../agent-side-integrations/scim/)           | The `userName` of the provisioned user |

The expression must match the whole username. For example `.*@example\.com` allows anyone with an example.com address and nobody else.

Users matching the [superusers](adding-a-superuser.md) expression are always allowed, whatever `allowed_usernames` contains.

### Existing users

The check runs every time a user is resolved from their sign-in method, so changing `allowed_usernames` affects users who already exist:

* Agents authenticating with a JWT are checked on every request. A `sub` that no longer matches is rejected immediately with the `invalid_username` error.
* Users signed in to the dashboard with a session cookie keep their session, and are rejected the next time they sign in.

### Setting allowed\_usernames

1. Login to your Cobrowse dashboard as a [superuser](adding-a-superuser.md).
2. Visit `admin/configuration` for example https://example.com/admin/configuration
3. Add a new config value with the key `allowed_usernames` and the value being your regular expression.
4. Save and restart the API service for the new value to take effect.
