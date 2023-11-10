#### OAUTH2.0
- delegated authorization
- access token and refresh token (temporary password and limited scope)
- if client is compromised, the access token can be revoked easily and not affected the password db
- authorization http header
- channel
  - back (very secured)
  - front (browser)
- oauth is not for authentication
- but it get abused, hence google and facebook login

#### Cookie and Session
- Session is transported by the cookies
- Session id is stored in the server system
- long live session vs short live session (inactiveness allowed)
- logged out and session id is deleted

#### Simple login
- username and password
- check in db
- verify hash password
- set cookie: session-id, max-age
- downsides: maintenance, security

#### SSO: Single Sign On
- able to get into many systems with one login
- SAML

#### oauth2.0 and oidc
[Good video](https://www.youtube.com/watch?v=996OiexHze0)

#### what oidc adds
- id token
- user info endpoint
- standard set of scopes
- standardized implementation
