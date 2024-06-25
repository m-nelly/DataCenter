---
Author(s): M-Nelly
Source: Internal
Subject: Security
Topic: Authorization
Description: Overview of OAuth2.0
Comments: 
tags:
  - security
---
## To-Do
- [x] Overview
- [ ] Security Concerns
- [ ] Implementation
- [ ] Common Mistakes
## References
- [OAuth Intro To IAM - What is OAuth2.0?](https://auth0.com/intro-to-iam/what-is-oauth-2)
- [PortSwigger Academy - JWT Attacks](https://portswigger.net/web-security/jwt)
- [IETF RFC 6749 - OAuth2.0 Authorization Framework](https://datatracker.ietf.org/doc/html/rfc6749)
- [Proof Key for Code Exchange (PKCE) in OAuth2 Authorization Code Grant](https://medium.com/@dckalubowila25132/proof-key-for-code-exchange-pkce-in-oauth2-authorization-code-grant-8b36d7e11d61)
- [Auth0 - Device Authorization Flow](https://auth0.com/docs/get-started/authentication-and-authorization-flow/device-authorization-flow)
## Notes
### Introduction
OAuth is an open authorization protocol (NOT an authentication protocol) which allows a website or application to access resources on behalf of a user. OAuth2.0 uses access tokens to represent the authorization to access resources on behalf of the end user. The most common implementation of the access token is the JSON Web Token (JWT). 
>[!Note] Information about JWT attacks can be found [here](https://portswigger.net/web-security/jwt). 
#### OAuth2.0 Roles
The idea of roles is part of the core specification of the OAuth2.0 authorization framework. These define the essential components of an OAuth 2.0 system, and are as follows:

- **Resource Owner:** The user or system that owns the protected resources and can grant access to them.
- **Client:** The client is the system that requires access to the protected resources. To access resources, the Client must hold the appropriate Access Token.
- **Authorization Server:** This server receives requests from the Client for Access Tokens and issues them upon successful authentication and consent by the Resource Owner. The authorization server exposes two endpoints: the Authorization endpoint, which handles the interactive authentication and consent of the user, and the Token endpoint, which is involved in a machine to machine interaction.
- **Resource Server:** A server that protects the user’s resources and receives access requests from the Client. It accepts and validates an Access Token from the Client and returns the appropriate resources to it.
#### Access Token Scope
I don't always recommend learning directly from RFCs; however, in this case I would recommend reading through the section on access token scope [here](https://datatracker.ietf.org/doc/html/rfc6749#section-3.3) or below:
>[!Important]
The authorization and token endpoints allow the client to specify the
scope of the access request using the "scope" request parameter.  In
turn, the authorization server uses the "scope" response parameter to
inform the client of the scope of the access token issued.
>
The value of the scope parameter is expressed as a list of space-
delimited, case-sensitive strings.  The strings are defined by the
authorization server.  If the value contains multiple space-delimited
strings, their order does not matter, and each string adds an
additional access range to the requested scope.
>
 scope       = scope-token \*( SP scope-token )
 scope-token = 1*( %x21 / %x23-5B / %x5D-7E )
>
The authorization server MAY fully or partially ignore the scope
requested by the client, based on the authorization server policy or
the resource owner's instructions.  If the issued access token scope
is different from the one requested by the client, the authorization
server MUST include the "scope" response parameter to inform the
client of the actual scope granted.
>
If the client omits the scope parameter when requesting
authorization, the authorization server MUST either process the
request using a pre-defined default value or fail the request
indicating an invalid scope.  The authorization server SHOULD
document its scope requirements and default value (if defined).
#### Access Tokens and Authorization Codes
Once the resource owner has authorized access, the OAuth server returns an *Authorization Code* to the client that can be used to get an *Authorization Token*. If needed, the authorization server can issue refresh tokens with the authorization token. The refresh token typically has a long expiry time and can be exchanged for a new access token before the original expires. Because of the inherent risk associated with the disclosure of a refresh token, the client must store refresh tokens securely. 
#### Grant Types
In OAuth2.0, grants determine the process the *client* has to go through to gain authorization to a resource.  
- Authorization Code - The *authorization server* returns a single-use *authorization coke* to the client which can be exchanged for an *authorization token*. 
- Authorization Code with PKCE - Similar to the Authorization Code grant; however, additional security measures are put in place to prevent a MitM from obtaining authorization. 
- Implicit - Access token is returned directly to the client. This grant type is deprecated. 
- Resource Owner Credentials - If the *client* is trusted by the *resource owner* and the authorizing entity, the *client* can accept and pass the user's credentials directly to the *authorization server* which eliminates a user redirect from the authorization flow. 
- Device Authorization Flow - Authorization flow which takes place on an input constrained device as well as a browser controlled by the *resource owner* in order to authorize *clients* such as smart TVs. 
- Refresh Token - This flow controls the exchange of a refresh token for a new *Access Token*. 
#### How Does It Work?
Let's walk through the process with an example. Let's say you are wanting to authorize a third-party service called Playlist-ABC to use your Spotify account to organize the songs in your playlists alphabetically. In this case, you are the *resource owner*, the  Playlist-ABC is the *client*, and Spotify owns the *authorization server*. 
- You, the *resource owner*, make a request to Playlist-ABC to re-order the songs in your playlists. 
- Playlist-ABC now has to initiate a request to Spotify for the access it needs. Playlist-ABC sends an *authorization request* to Spotify's *authorization server* for authorization to access the *scope* which includes your playlists and basic account information.  
- Spotify's *authorization server* authenticates the *client*, Playlist-ABC, and verifies that the requested *scopes* are permitted. 
- You, the *resource owner*, receive a request to grant Playlist-ABC access to the requested *scopes*. 
- You grant access and the *authorization server* sends the *client* an *authorization code* or an *access token* based on the *grant type*. A refresh token will not be included in this case because account access is only needed for a short period of time; however, in some other cases this might be included in the response. 
- Using the *access token*, Playlist-ABC is able to perform the requested actions on your behalf. 
### Security Concerns
- Using the *Authorization Code* grant type without PKCE on mobile devices may lead to a leak of the AC to a malicious application on the device.
- Implicit grants are vulnerable to MitM attacks. 
