<!-- Content From PWAPT Course Notes. Will remove/modify later. -->
Web App Types
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
Burp: Engagement Tool - Discover Content
- Directory brute force.
- Provides sitemap of results. 
- `FuzzDB` and `seclists` are solid wordlists
Exposed Admin Interfaces:
- Tomcat `/manager/html`
- WordPress `/wp-admin`
Monitoring & Logging
- `/server-status` 
- `/elmah.axd`
HTTP X headers usually disclose useful information.
CORS is a server-controlled mechanism to bypass Same-Origin Policy.
Permissive CORS policies allow a bypass of the SOP to some extent. Sometimes even complete bypass of the SOP. 
Mismatched Content Types - Request
- Accepting a request with a content type that doesn't match the received payload. 
	- `text/plain` vs `application/json`
Authentication System Analysis Workflow:
1. Work through auth in the browser
2. Review auth in the proxy
3. ID auth types used
4. ID auth schemes
5. Test implemented auth schemes
6. ID types of temp creds 
7. ID how temp creds are managed
8. Describe the auth system
Common Authentication Systems:
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
Session Fixation
- A form of session hijacking
- Impersonate a user by luring them to authenticate with a known session
- Results from failure to issue new sessions on login
- Requires XSS, cookie-less sessions, or physical access to exploit
- Improperly scoped cookies permit attack via XSS through any app in the same domain
Injection Testing
- Effectively reverse engineering
	- Determine server-side logic given known I/O
	- Hypothesize what might be incorrectly handled & test using automated tools
	- Manually test if scanners don't find anything or if dealing with a sensitive app
- Scanners are good, understanding of how web apps work is essential
- Which inputs appear to reference local files?
	- Directory traversal, LFI/RFI
- Which inputs appear to reference third-party resources?
	- SSRF (Server-Side Request Forgery)
- Does the application authenticate individual requests for sensitive transactions?
	- CSRF (Cross-Site Request Forgery)
- Does the application accept POST payloads as query strings in a GET request?
	- Method Interchange
- Which inputs are dynamically added to the page by client-side code?
	- D-XSS (DOM-Based Cross-Site Scripting)
- Which inputs appear to be attributes for the creation of a model object?
	- Mass Assignment: Ruby on Rails, NodeJS
	- Auto-binding: Spring MVC, ASP .NET MVC
	- Object Injection: PHP
- Do any requests accept XML payloads?
	- XXE (XML External Entity Injection)
- Which inputs contain serialized data in a non-traditional format?
	- Insecure Deserialization
- Which inputs contain file data that is stored by the server?
	- Malicious File Uploads, Web Shells, Implants
Business Logic
- A class of vulnerability, not a specific flaw
	- Depends on functionality provided by the application
- Requires knowledge and understanding of the business
- Functional analysis is critical to finding these types of issues
	- Core business processes
	- Objectives of the application
Query String Parameters
- Parameters included in the URL of a request
	- Usually GET, but can also exist in POST requests
- URLs persist in many places
	- Intermediate proxies
	- Server logs and status pages
	- Browser cache and bookmarks
	- Referer headers (Cross-Domain Leakage)
	- Third-Party analytics 
	- Copy/Paste/Share
	- MacOS "where from" metadata tag
- Sensitive information passed in this manner could be exposed
Well-Known URIs
- Standardized list of suffixes combined with a "well-known" URI path prefix
	- RFC8615 reserves the path prefix `/.well-known` 
	- https://www.iana.org/assignments/well-known-uris/well-known-uris.xhtml
	- Supports site-wide metadata discovery
TLS Stripping
- MITM Technique
- Attacker maintains HTTP connection with client and HTTPS connection with the server
- Browser requests content via HTTP before being redirected to HTTPS
- Resulting behavior is browser dependent
	- Missing lock, note regarding security
	- More stealthy than untrusted cert warnings
Mismatched Content Types - Response
- Responding with a content type that doesn't match the returned payload
	- `text/plain` vs. `application/json` for JSON, etc. 
	- May result in XSS if the content type is rendered as HTML
	- `text/html` for all browsers
	- `text/plain` for legacy IE
Serialization
- The process of converting complex data structures into a format that can be transmitted as a sequential stream of bytes
- Persists the state of the object (properties and values)
- String Formats: JSON, XML, YAML, etc. 
- Binary Formats: BSON, Message Pack, etc. 
- Both formats are compatible with HTTP
- Also known as marshalling (Ruby) and pickling (Python).
- Deserialization is the reversal of this process resulting in a fully functional replication of the original object