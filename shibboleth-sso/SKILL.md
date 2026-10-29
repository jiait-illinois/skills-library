---
name: shibboleth-sso
description: Implement or refactor University of Illinois Shibboleth SSO using the workspace's existing OAuth/OpenID Connect stack when present, otherwise the user-specified library, otherwise the library that best fits the repository's web framework or software architecture.
argument-hint: Existing auth stack, preferred OAuth/OIDC library if any, and the target app or route files to update
---

# Shibboleth SSO

Use this skill to add, update, or review University of Illinois Shibboleth single sign-on using OpenID Connect.

Treat the repository's existing auth approach as the primary anchor. Keep the implementation aligned with the current framework, routing style, and session model instead of introducing a parallel authentication system.

## Support

Applications must be registered by the Identity & Access Management Team to
enable Shibboleth SSO. Contact shibboleth-mgr@illinois.edu for assistance with
Shibboleth integration.

## Defaults

- OIDC issuer / base endpoint: `https://shibboleth.illinois.edu`
- OIDC discovery document: `https://shibboleth.illinois.edu/.well-known/openid-configuration`
<!-- TODO: Use OIDC-specific logout endpoint once newer logout functionality is implemented (https://shibboleth.illinois.edu/idp/profile/oidc/logout) -->
- Logout endpoint: `https://shibboleth.illinois.edu/idp/profile/Logout`
- Default scopes: `openid profile email`
- Default redirect URI path: `/auth/callback`
<!-- TODO: Verify this works with Shib server config update -->
- Default client ID pattern, unless the user specifies otherwise: `myservice.myunit.illinois.edu`

## Library Selection

Choose the OAuth / OpenID Connect library in this order:

1. Use the library already installed in the workspace, if one is present.
2. Otherwise use the library explicitly provided by the user.
3. Otherwise choose the library that best matches the repository's existing web framework or architecture.

When the best choice is not obvious, inspect the dependency manifests, lockfiles, existing auth middleware, and current session handling before deciding.

## Required Claims

Submit required claims only if explicitly requested by the user **AND** they're supported by the selected OIDC library, eg.:
```json
{
  "userinfo": {
    "eduperson_principal_name": {"essential": true},
    "preferred_username": {"essential": true},
    "uiucedu_uin": {"essential": true}
  }
}
```

## Session Mapping

<!-- TODO: NetID won't be available on the id_token until Keith updates production config in a couple weeks -->
Unless the user explicitly says to persist different ID token attributes, map the identity values available in the ID token userinfo claim set onto the client session user object as follows:
- `preferred_username` -> `netid`

If the user requests to save any of the following attributes on the client session user object, they must be fetched from the userinfo endpoint and mapped accordingly:
| Client Session Attribute | UserInfo Endpoint Attribute |
| ------------------------ | --------------------------- |
| `uin`                    | `uiucedu_uin`               |
| `netid`                  | `preferred_username`        |
| `email`                  | `eduperson_principal_name`  |
| `name`                   | `name`                      |
| `first_name`             | `given_name`                |
| `middle_name`            | `middle_name`               |
| `last_name`              | `family_name`               |
| `eduperson_affiliation`  | `eduperson_affiliation`     |

Avoid sending a UserInfo request unless required by the user's specifications.

## Credential Handling

- Do not hardcode non-empty defaults for the OIDC client ID or client secret.
- Treat the client secret as sensitive configuration and pass it through secure application configuration, typically environment variables.
- For mobile apps or cases when the client secret has to be available on the client's device, use the OIDC PKCE extension instead of a static client secret.
- If a sample secret is needed for local setup, use a random string of at least 32 characters, but never bake it into committed runtime defaults.

## Procedure

1. Inspect the repository for the existing auth stack.
	- Look for installed OAuth / OIDC libraries in package manifests, lockfiles, and source imports.
	- Identify the app framework, session mechanism, and current login / callback / logout routes.
	- Prefer the existing auth flow over adding a second one.
2. Select the library.
	- Reuse the installed library when possible.
	- Otherwise honor the user's explicit preference.
	- Otherwise pick the smallest library that fits the app's framework and session model cleanly.
3. Configure the provider.
	- Use the Shibboleth discovery document.
	- Apply the default scopes unless the user overrides them.
	- Request the required claims above when no other requirement set is provided.
	- Set the callback path to `/auth/callback` unless the user specifies another route.
4. Wire the session.
	- Map `preferred_username` to `netid` in the client session user info unless told otherwise.
	- Keep the minimum user profile data needed by the app.
	- Avoid introducing a dependency on the userinfo endpoint unless explicitly required.
5. Implement sign-in and sign-out.
	- Add the login initiation route or handler in the repository's existing style.
	- Add the callback handler for `/auth/callback`.
	- Use the Shibboleth logout endpoint for sign-out when supported by the chosen library and app architecture.
6. Validate the change.
	- Confirm the discovery metadata is used correctly.
	- Confirm the client ID and secret are sourced from configuration rather than hardcoded.
	- Confirm the required claims, callback route, and session mapping match the requested defaults.

## Decision Points

- If the workspace already includes a suitable OAuth / OIDC dependency, do not introduce a new one without a clear reason.
- If the app framework already has a canonical auth integration pattern, follow it even if a different library could also work.
- If the user specifies a library, route, claim set, or session mapping, follow the user over the defaults in this skill.
- If the app cannot support OIDC logout cleanly, implement the closest safe logout behavior and note the limitation.
- If the workspace uses a backend-for-frontend or server-side session model, keep the OIDC tokens off the client unless the repository already does otherwise.

## Quality Bar

The implementation is complete when:

- The selected OAuth / OIDC library matches the workspace, user preference, or framework fit in that order.
- Discovery uses `https://shibboleth.illinois.edu/.well-known/openid-configuration`.
- The login flow defaults to `openid profile email` scopes.
- The client ID and client secret are not hardcoded as non-empty defaults.
- Required claims are omitted unless requested by the user and they're supported by the OIDC library.
- The callback route defaults to `/auth/callback` unless overridden.
- The app does not call the UserInfo Endpoint unless required by the user's client side session user object specifications.

## Example Invocations

```text
Add Shibboleth SSO to this app using the installed OIDC library if one exists. Default to openid profile email scopes, request preferred_username and uiucedu_uin, use /auth/callback, and keep client credentials in environment variables.
```

```text
Update the current auth flow for University of Illinois Shibboleth login. Use the repo's existing OAuth/OIDC stack if present, avoid the userinfo endpoint unless necessary, and map the returned identity claims into the session user object.
```
