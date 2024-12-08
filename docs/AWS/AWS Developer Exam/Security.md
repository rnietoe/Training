# Security

## Cognito

**`AWS Cognito`** is the SSO Solution in AWS providing Web Identity Federation (auth using **Facebook**, **Google** and Amazon) recomended for mobile apps log in

store user profiles

based on open standards as **OAuth2.0**, SAML2.0, OpenID Connect

1. users pool: **JWT** (Json Web Token) for registration, authentication and account recovery
2. identity pool: temporary AWS credentials to access AWS recources. 

Cognito uses Push synchronization to push updates and synchronize user data across multiple devices