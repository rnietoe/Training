# Web Security: OAuth and OpenID Connect

[https://www.linkedin.com/learning/web-security-oauth-and-openid-connect?u=1009514](https://www.linkedin.com/learning/web-security-oauth-and-openid-connect?u=1009514)

* [OAuth 2.0 Servers](https://www.oauth.com)
* [Map of OAuth 2.0 Specs](https://www.oauth.com/oauth2-servers/map-oauth-2-0-specs)
* Aaron Parecki's book **OAuth 2.0 Simplified**

## What is OAuth?
Some OAuth Servers:

* https://oauth2.thephpleague.com 
* https://developers.google.com/oauthplayground 
* https://openidconnect.net 
* **https://developer.okta.com** 

A token introspection tool: https://www.jsonwebtoken.io 

* flow = grant type
* scopes = permissions

AuthZ with:

* API keys
* Id & Secret
* OAuth

OpenID Connect (OIDC) is an OAuth 2.0 identity extension – only for users.

## Core Terminology
Main OAuth extensions

* **/authorize**: grant permission to the resource. Could return an authZ code or an access token
* **/token**: trade an authZ code or refresh token for an access token 
* **/revoke**: deactivate (invalidate) a token. Valid for access or refresh tokens
* **/instrospect**: learn more about the token 
* **/register**: create new OAuth clients
* **/userinfo** : retrieve profile information about the authenticated user
* **/.well-known/openid-configuration**: which endpoints are support by our OAuth server

Main OAuth tokens

* Access token: gives the client application access to the protected resource, usually the API
* Refresh token: used to request a new access token when this expires
* ID Token (included with OIDC): user profile information 

JWT (Json Web Token)

* **iss**: the issuer of the token, an entity we trust
* **sub**: the subject (user) of the token
* **aud**: the audience which should consume the token
* **exp**: expiration time of the token
 
Claims: a key/value pair within the token that gives the client application information

## Client Credentials: AuthZ for microservices
Private clients only where secrets are in backend code; not appropiate for web  pages or mobiles apps

* Must use secure communications TLS
* no user relationship
* validate access token against the **/instrospect** endpoint
* expired token has to be rejected

## Implicit or Hybrid: AuthZ for mobile devices
Attach the user to the process

* cookie/session storage
* AppAuth and Passport for Android or iOS
* Must use secure communications TLS
* use of **redirect_uri**

## Authorization Code
get an Auth Code instead of an access token. 

* The third-party application never sees our credentials. 
* The end user never sees the access token.

## Resource Owner Password Flow

* recommended **only** for updating legacy systems
* danger! credentials are managed by the application itself

## Server side implementations


