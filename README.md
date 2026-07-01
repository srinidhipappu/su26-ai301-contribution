# Contribution [1]: @nuxt/auth integration for Asgardeo #364


**Contribution Number:** 1
**Student:** Srinidhi Pappu
**Issue:** https://github.com/asgardeo/javascript/issues/364 

The issue is essentially looking for an official Asgardeo authentication system for Nuxt.js so developers can easily add Asgardeo login to their applications without manually configuring OAuth/OpenID Connect settings every time. In order to solve this issue, I would need to build a new authentication sysyem that handles login, logout, token management, PKCE security, and user profile retrieval, similar to how Auth0 is already integrated into Nuxt Auth.

**Status:** Phase I Complete

---

## Why I Chose This Issue

I chose this issue because it aligns closely with my interests in software engineering, fintech, authentication systems, and web dev. Asgardeo is an identity and access management platform, and working on an official Nuxt.js authentication provider would give me hands-on experience with industry-standard technologies such as OAuth 2.0, OpenID Connect (OIDC), PKCE, and token management. I am also interested in learning how authentication SDKs are designed and integrated into modern web frameworks, and working on this issue for the next 4 weeks would give me an opportunity to contribute a meaningful feature that improves the developer experience for Nuxt users while expanding my knowledge of secure application development and open-source collaboration.

---

## Understanding the Issue

### Problem Description

The authentication system currently does not have an Asgardeo provider. Although Asgardeo supports OpenID Connect (OIDC) and can be integrated using a generic OIDC strategy, developers must manually configure endpoints, PKCE settings, token handling, and logout behavior. This leads to extra setup efforts and inconsistent implementations across different projects.

### Expected Behavior

The authentication module should include an official Asgardeo provider that developers can navigate with minimal settings (such as clientId and issuer). The provider should automatically handle OIDC discovery, PKCE, token management, user profile retrieval, and logout functionality, similar to existing providers like Auth0.

### Current Behavior

Currently, there is no built-in Asgardeo provider available. Developers who want to use Asgardeo must set up a generic OIDC strategy manually, including authentication endpoints, token handling, PKCE configuration, and logout settings, which is much more tedious and can lead to errors while configuring.


### Affected Components

[Which parts of the codebase are involved?]

This issue occurs within two codebases:
The actual @nuxt/auth provider implementation belongs in the upstream nuxt-community/auth-module repo, likely under src/providers.
This local Asgardeo JavaScript SDK repo contains the Asgardeo/Nuxt behavior you would use as reference material, especially packages/nuxt, packages/javascript, and packages/node.


---

## Reproduction Process

### Environment Setup

[Notes on setting up your local development environment - challenges you faced, how you solved them]

### Steps to Reproduce

Since this is a feature request:
Step 1: Create a Nuxt application that uses Nuxt Auth.
Step 2: Create or configure an Asgardeo application and attempt to integrate it with Nuxt Auth.
Step 3: Look for a built-in Asgardeo strategy/provider.
Step 4: Notice that that no Asgardeo provider exists and that a generic OIDC strategy must be configured manually.
Result: There is no dedicated Asgardeo provider available. Developers must manually configure a generic OIDC strategy, including issuer information, authentication endpoints, PKCE settings, token handling, and logout behavior.


### Reproduction Evidence
- I discovered that in the process of trying to implement Asgardeo into a new test Nuxt application, the module did not exist and could not be recognized. 

---

## Solution Approach

### Analysis

The issue exists because there is currently no dedicated Asgardeo provider implementation available for Nuxt Auth integration. While Asgardeo supports OpenID Connect (OIDC), developers must use a regular OIDC strategy and manually set up authentication endpoints, PKCE settings, token handling, user profile retrieval, and logout behavior.

### Proposed Solution

Implement an official Asgardeo provider that follows the same patterns used by existing authentication providers.

The provider should:
- Accept minimal configuration such as clientId and issuer
- Automatically perform OIDC discovery
- Enable PKCE by default
- Handle access tokens, ID tokens, and refresh tokens
- Retrieve user profile information from the UserInfo endpoint
- Support logout functionality
- Provide documentation and examples for Nuxt developers

This would eliminate the need for manual OIDC configuration while providing a consistent integration experience.


### Implementation Plan

Understand:
The current authentication ecosystem does not provide a dedicated Asgardeo provider. Developers can authenticate through Asgardeo using generic OIDC support, but they must manually configure several authentication-related settings.
The expected behavior is to provide an Asgardeo provider that automatically handles common OIDC functionality and requires minimal setup.

Match:
I plan to review existing provider implementations that already solve a similar problem. That way, these implementations can be used as templates for the new Asgardeo provider.
I plan to review sources such as:
    - Auth0 provider
    - Other OIDC-based providers
    - Existing provider registration patterns
    - Existing authentication documentation structure
    

Plan
    1. Investigate the provider architecture and identify how providers are registered.
    2. Locate existing OIDC-based provider implementations and review their structure.
    3. Create a new Asgardeo provider implementation using the existing provider pattern.
    4. Configure OIDC discovery using the supplied issuer URL.
    5. Enable PKCE support by default.
    6. Add token handling and refresh token support.
    7. Implement user profile retrieval through the UserInfo endpoint.
    8. Implement logout support.
    9. Add documentation and configuration examples.
    10. Add or update automated tests for the new provider.

Expected files to modify (subject to repository structure):
    - Provider implementation files
    - Provider registration/configuration files
    - Documentation files
    - Authentication test files

Implement
    Branch link: https://github.com/srinidhipappu/javascript/tree/nuxt/auth-integration
    Fork link: https://github.com/srinidhipappu/javascript
    Commits and branch links will be added during development.

Review
    Before submitting the pull request I plan to:
        - Review CONTRIBUTING.md
        - Verify code style requirements
        - Follow project commit message conventions
        - Follow project pull request conventions
        - Ensure documentation is updated
        - Ensure tests pass successfully
        - Confirm no existing provider functionality is affected

Evaluate

 Verification will include:
        - Functional Verification
        - Configure a sample application using the new Asgardeo provider.
        - Authenticate successfully using an Asgardeo tenant.
        - Verify token retrieval and storage.
        - Verify user profile retrieval.
        - Verify logout functionality.
        - Automated Testing
        - Add tests covering provider configuration.
        - Add tests covering OIDC discovery behavior.
        - Add tests covering PKCE defaults.
        - Add tests covering user profile mapping.
        - Run the project's existing test suite to ensure no regressions.

 Success Criteria:
    Developers can integrate Asgardeo using a dedicated provider configuration without manually configuring OIDC endpoints and related    authentication settings.


---

## Testing Strategy

### Unit Tests

- [x] Verified extracted configuration resolves the expected Asgardeo runtime configuration values (baseUrl, clientId, afterSignInUrl).
- [x] Verified `client.initialize(resolvedConfig)` receives the extracted configuration without changing initialization behavior.
- [x] Verified the generated authorization URL contains the expected `redirect_uri` (`http://localhost:3000/api/auth/callback`).

### Integration Tests

- [x] Completed the OAuth sign-in flow using the Nuxt sample application.
- [x] Verified the callback exchanges the authorization code, establishes a session, and renders the authenticated user.

### Manual Testing

- Ran `nx typecheck @asgardeo/nuxt` successfully.
- Ran `nx build @asgardeo/nuxt` successfully.
- Configured a local Asgardeo application with the correct redirect URI.
- Logged into the Nuxt sample application through Asgardeo.
- Confirmed successful authentication by seeing the authenticated landing page (`Welcome back, <user>`), verifying that the configuration extraction did not change the existing authentication flow.

---

## Implementation Notes

### Week [3] Progress

This week, I researched how to build an Asgardo provider, such as Auth0. Companies can use Asgaro handle:
    - Login
    - Signup
    - Password reset
    - Google/Microsoft SSO
    - User accounts
    - OAuth/OIDC

I realized that I neeeded to research how an Auth0 system works, since I am developing the feature from scatch. 

The pipeline I need to develop for this feature is:

User
 ↓
Clicks Login
 ↓
Redirected to Asgardeo
 ↓
Logs in
 ↓
Gets Authorization Code
 ↓
App exchanges code for tokens
 ↓
Access Token received

This was my research plan:

OpenID Connect (OIDC): 

OAuth answers: "Can this user access something?"
OIDC answers: "Who is this user?"

Learn:
openid scope
id_token
userinfo endpoint
issuer

Questions:
How does the app get a user's email?
What is an ID token?
What is the userinfo endpoint?

PKCE: 
Code Verifier
↓
Code Challenge
↓
Secure Token Exchange

Question:
Why does PKCE exist?

Just understand:
PKCE prevents someone from stealing the authorization code.

4. Nuxt Auth Architecture

Research:
Nuxt Auth Provider
Nuxt Auth OAuth Provider
Nuxt Auth Auth0 Provider

Questions:
How are providers registered?
How are strategies configured?
How do existing providers work?
5. Asgardeo Documentation

Read only:
Login flow
OIDC support
UserInfo endpoint

Some articles I researched:
https://openid.net/developers/how-connect-works/
https://auth0.com/docs/get-started/authentication-and-authorization-flow/authorization-code-flow-with-pkce
https://wso2.com/identity-platform/docs/guides/authentication/oidc/
https://auth.nuxtjs.org/
https://auth.nuxtjs.org/providers/auth0/

I ultimately learned:

When a user clicks "Login with Asgardeo," they should be redirected to Asgardeo's login page, where they authenticate and receive an authorization code. The application then exchanges this code for access, ID, and refresh tokens. The access token is then used to access protected resources, and the ID token contains information about the authenticated user, and the refresh token can obtain new access tokens when they expire. OpenID Connect extends OAuth by adding identity information through the UserInfo endpoint and ID tokens. PKCE (Proof Key for Code Exchange) adds an extra security layer by generating a code verifier and code challenge to prevent authorization code interception attacks. The purpose of the Asgardeo provider is to automate this entire flow within Nuxt Auth so developers only need to provide the basic configurations, such as clientId and issuer, rather than manually configuring OIDC endpoints, token handling, PKCE settings, user profile retrieval, and logout functionality. Ultimately, the provider acts as an integration layer between Nuxt Auth and Asgardeo, handling authentication, token management, user information retrieval, and session lifecycle management behind the scenes.

### Week [4] Progress

This week I continued implementing and validating the Nuxt authentication flow using the local sample application. 

During development, I extracted the Asgardeo server configuration into a dedicated configuration module while preserving the existing authentication flow. 

I confirmed that the resolved configuration was still passed into `client.initialize()` and that the downstream authentication logic remained unchanged.

While testing, I encountered several authentication issues that required debugging:

- The Asgardeo client was not initializing because required runtime configuration values (`baseUrl` and `clientId`) were missing.

- I configured the required environment variables using a local `.env` file and verified that the SDK correctly resolved the expected values.

- I then encountered an OAuth callback mismatch (`callback.not.match`) while authenticating with an Asgardeo sercer. To debug this, I temporarily added logging to inspect the generated authorization URL and verify the exact `redirect_uri` being sent to Asgardeo.

- The generated authorization URL correctly used: http://localhost:3000/api/auth/callback

- After comparing this value with the application configuration in the Asgardeo Console, I discovered that the authorized redirect URI had not actually been persisted in the application settings. Once the redirect URI was correctly saved, the OAuth flow completed successfully.

After correcting the application configuration, I successfully verified the complete authentication flow:

- User redirected to the Asgardeo login page.
- Successful authentication using a test user.
- Authorization code returned to `/api/auth/callback`.
- Authorization code exchanged for tokens.
- Session established successfully.
- Authenticated user information rendered correctly in the sample Nuxt application.

The successful end-to-end login confirmed that the configuration extraction did not change the inputs passed into `client.initialize()` and that the authentication pipeline, callback handling, session creation, and SSR hydration continued to function correctly after the refactor.

This debugging process also improved my understanding of OAuth 2.0, OpenID Connect, PKCE, runtime configuration, callback validation, and the importance of correctly configuring redirect URIs within an identity provider.


## Pull Request

**PR Link:** [GitHub PR URL when submitted]

**PR Description:** [Draft or final PR description - much of the content above can be adapted]

**Maintainer Feedback:**
- [Date]: [Summary of feedback received]
- [Date]: [How you addressed it]

**Status:** [Awaiting review / Iterating / Approved / Merged]

---

## Learnings & Reflections

### Technical Skills Gained

[What you learned technically]

### Challenges Overcome

[What was hard and how you solved it]

### What I'd Do Differently Next Time

[Reflection on your process]

---

## Resources Used

- [Link to helpful documentation]
- [Tutorial or Stack Overflow post that helped]
- [GitHub issues or discussions that helped]
