---
Author(s): 
Source: Internal
Subject: 
Topic: 
Description: 
Comments: 
tags:
---
## To-Do
- [ ] 
## Notes
### Web App Types
- Single Page - Interface is in a single page, client-side, and does not require a complete DOM refresh. 
- Server-Side - Rendered HTML originates from the server and is delivered via HTTP. 
- Client-Side - Rendered HTML originates from JS returned via HTTP and the page is built dynamically using direct access to the DOM. 

Web Server - Serves static content
App Server - Serves dynamic content
Reverse Proxy - Proxies HTTP requests from remote hosts to the backend server

Front Controller - Uses a single server-side page for all functionality. 
`http://example.com/index?page=catalog&item=42`
- File portion of the URL behaves like a wrapper containing shared UI elements and a dynamic content block
- `page` determines the high-level dynamic content rendered on the page
- `item` determines the low-level dynamic content rendered on the page 
- Served from the file system
- Navigation requires DOM refresh

Page Controller - Uses a separate server-side rendered page for each function. 
`http://example.com/catalog?item=42`
- Most common approach
- File portion of the URL determines high-level dynamic content
- Query portion of the URL determines the low-level dynamic content
- Served from the file system
- Navigation requires DOM refresh

Model View Controller - Allows developers to work on different parts of the app simultaneously without interference. 
- Easier version control
- SEO-friendly URLs
- Increased need for access control

Server-Side Model View Controller
`http://example.com/catalog/item/42`
- Does not conform to URI specification
- Served from memory
- Navigation requires server side DOM refresh
- Separates resources into 3 logical components
	- Model - Data modeling and interfacing
	- View - UI
	- Controller - Request handling and business logic

Client-Side Model View Controller
`http://example.com/#/catalog/item/42`
- Served from memory
- **Never** requires a DOM refresh from the server
- View portion of the MVC is moved to the client 
- Data is passed from the controller to the view via REST API
- *Major implications for access controls in the UI*
### Authentication Schemes
- Server-Side rendered application
	- Forms auth requiring a native username and password - implicitly managed session
- Client-Side rendered application with exclusive API
	- Forms auth requiring native username and password - explicitly managed token
- Client-Side rendered and/or mobile application with shared backend API
	- Forms auth requiring native username and password - explicitly managed token
- API in microservices architecture
	- Developer portal issues explicitly managed token

HTTP Integrated Authentication
- Auth protocol built into HTTP
- Challenge/Response with WWW-Authenticate/Authorization headers. 
	- Browser caches credentials
- Keyword determines scheme & parameters: `Basic`, `Digest`, `NTLM`, `Negotiate`, etc. 
- No native support for credential recovery or rotation
- No support for credential timeout or logout.
- Implicit auth introduces CSRF risk.

Most sites will used some other form of `forms` based authentication that will most likely be based on custom code or some type of standard like OpenID Connect or SAML. 

Temp Creds
- Continuous reauthentication over stateless protocol (HTTP)
- Prevents users from having to reauthenticate for each request
- Reduces the risk of exposing permanent credentials
- Tokens are stateless (server does not store any information about the user)
- Session cookies are stateful

Tokens
- Serialized object containing name: value pairs
	- Usually B64, JSON, or JWT 
- Operates within stateless nature of HTTP
- Contains everything required to reauthenticate
- Stored only on the client
- Potential for disclosure or theft 
	- Transparent by default
	- Contains sensitive information
- Credential logout is not supported
	- Server maintains no knowledge of the token
- Increased potential for key exposure
	- Scaling requires shared key to verify signatures

Session 
- Credentials are opaque values stored on the client and server
- State information is stored on the server with the credential
- State contains transactional information and everything required to reauthenticate
- Sensitive information is stored on the server
- Credentials can be revoked at any time
- Credentials can be guessed if passwords are not properly hashed and salted
- More difficult to implement
- Scaling increases complexity
- Higher processing load

Temporary Credential Management
- Implicit - Browser Cookies
- Explicit - Client-Side Code
- Not bound to a credential type

Implicit Credential Management
- Credentials are handled by the browser
	- Uses cookies and does not require input from the app or user
- Cookies can be removed from the DOM which prevents theft via XSS
- Cookies default to a safe expiration
- Browsers contain built-in protections for cookies
- Introduces possibility of CSRF
- Cookies can be loosely scoped which leads to Session Fixation vulnerabilities

Explicit Credential Management
- Credentials are handled manually by the application
	- Requires custom code to store and revoke credentials
	- Requires custom code to apply credentials to requests
- Removes possibility of CSRF
- Client-side storage is tightly scoped to the origin
- Exposes credential to the DOM which introduces risk of credential theft via XSS
- Client side storage has no expiration so credentials must be manually removed
## References
### External
- 
### Internal
- 