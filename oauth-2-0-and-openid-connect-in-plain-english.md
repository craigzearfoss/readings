# OAuth 2.0 and OpenID Connect (in plain English)

- OktaDev
- Speaker: Nate Barbettini
- [https://www.youtube.com/watch?v=996OiexHze0&t=1734s](https://www.youtube.com/watch?v=996OiexHze0&t=1734s)
- Slide: [https://speakerdeck.com/nbarbettini/oauth-and-openid-connect-in-plain-english](https://speakerdeck.com/nbarbettini/oauth-and-openid-connect-in-plain-english)
- Feb 5, 2018
- Completed Apr 15, 2020

---

#### [OAuth 2.0 <debugger>](https://oauthdebugger.com/) - Tool for testing OAuth 2.0 requests and debug responses.

#### [OpenID Connect <debugger>](https://oidcdebugger.com/) - Tool for testing OpenID Connect requests and debug responses.

# Downside of doing authentication inside of your app:

    - Security
    - Maintenance

### **OAuth 2.0** and **OpenID Connect** are becoming the industry best practices for solving these problems.

### A lot of confusion around OAuth.

- A lot of terminology and jargon.
- Incorrect advice.

## Identity use case (pre-2010)

- Simple login (forms and cookies)
- Single sign-on access sites (SAML)
- Mobile app login (???)
- Delegated authorization (???) - How can I let a website access my data without giving it my password?

### Delegated authorization with OAuth 2.0

_OAuth 2.0 Authorization Code Flow_

```
 Resource
  Owner
    |
    V     Client                                            Authorization Server
+-------------------------+                                 +---------------------+
|        yelp.com         |                                 | accounts.google.com |
|                         | Go to authorization server      | +-----------------+ |
| +---------------------+ |================================>| | Email           | |
| | Connect with Google | | Redirect URI: yelp.com/callback | +-----------------+ |
| +---------------------+ | Response type: code             | +-----------------+ |
+-------------------------+ Scope: profile contacts         | | Password        | |
                                                       +--> | +-----------------+ |
                      Exchange authorization code      |    +---------------------+
                   +-----------------------------------+              ||
                   |  for an access token                             || Request consent
                   V                                                   V from resource owner
+-------------------------+                                 +---------------------+
|    yelp.com/callback    |                                 | accounts.google.com |
|                         |   Back to redirect URI          |                     |
|        Loading...       |<================================| Allow Yelp to access|
|                         |   with authorization code       | your public profile |
+-------------------------+                                 | and contacts?       |
     ^                                                      |     +-----+ +-----+ |
     |                                                      |     | Yes | | No  | |
     | Talk to resource server                              |     +-----+ +-----+ |
     | with access token                                    +---------------------+
     V
+---------------------+
| contacts.google.com |
+---------------------+

== (double lines) - front channel (you can see the query request parameters)
-- (single line)  - back channel (does not happen through the browser)
                    Uses a secret key which we don't ever want to expose to the browser.
```

## OAuth 2.0 terminology

- **Resource owner** - The user at the computer who owns the data that the application wants to get to.
- **Client** - The application that wants to get to the data.
- **Authorization server** - The system that can authorize something to happen.
- **Resource server** - The system that hodls the data that the client wants to get to.
- **Authorization grant** - The thing that proves that the user has click "yes" to grant authorization.
- **Redirect URI** - Where does the user go at the end of the flow.
- **Access Token** - The key that is used to get into the data that the client has been granted access to on the resource server.
  - Considered to be sensitive information so it wis done on the back channel.
- **scope** - The authorization server has a list of scopes (permissions) that it understands.
- **consent** - Screen presented to the user asking if they want to grant to that particular level of scopes.
- **back channel** - A highly secure channel. (like server making an api request)
  - The last step of the flow happens on the back channel.
- **front channel** - A less secure channel. (like a browser)
  - Used to interact with the user.

### The access token is scoped to the level of access that was requested in the beginning.

### Why do we get a code instead of just getting the access token right away?

- It takes advantage of the best things about the back channel and the the front channel.

### You need to create a client with the authorization server.

- You'll get two things that will be used to identify you to the authorization server.
  1. A client id
  2. A client secret
- The client secret is used in the back channel request for the access token.

## The Flow:

1. **Starting the flow**

```
https://accounts.google.com/o/oauth2/v2/auth?
    client_id=abc123&
    redirect_uri=https://yelp.com/callback&
    scope=profile&
    response_type=code&
    state=foobar
```

2. **Calling back**

```
https://yelp.com/callback?
    error=access_denied&
    error_description=The user did not consent.

OR

https://yelp.com/callback?
    code=oMsCeLvIaQm6bTrgto7&
    status=foobar
```

3. **Exchanging code for an access tocken**

```
POST www.googleapis.com/oauth2/v4/token
Content-Type: application/x-www-form-urlencoded

code=oMsCeLvIaQm6bTrgto7&
client_id=abc123&
client_secret=secret123&
grant_type=authorization_code
```

4. **Authorization server returns an access token**

```
{
    "access_token": "fFAGRNJru1FTz70BzhT3Zg",
    "expires_in": 3920,
    "token_type": "Bearer",
}
```

5. **Use the token**

```
GET api.google.com/some/endpoint
Authorization: Bearer fFAGRNJru1FTz70BzhT3Zg

+--------+     Token     +---------+ - Validate token
| Client |==============>|   API   | - Use token scoper for authorization
+--------+               +---------+
```

Client holds onto the access token and attaches it on subsequent requests.

### Sometimes **access** tokens are called **bearer tokens**.

## OAuth 2.0 Flows

- **Authorization code** - front and back channel
- **Implicit** - front channel only
  - Just get the access token back from the authorization server.
  - Sometimes used in pure JavaScript apps that have no back end.
  - A little less secure because the token is exposed to the browser.
- **Resource owner password credentials** - back channel only
  - Sometimes used t make older applications work correctly.
- **Client credentials** - back channel only
  - Sometimes used for machine-to-machine or service communication.

_OAuth 2.0 Implicit Flow_

```
 Resource
  Owner
    |
    V     Client                                            Authorization Server
+-------------------------+                                 +---------------------+
|        yelp.com         |                                 | accounts.google.com |
|                         | Go to authorization server      | +-----------------+ |
| +---------------------+ |================================>| | Email           | |
| | Connect with Google | | Redirect URI: yelp.com/callback | +-----------------+ |
| +---------------------+ | Response type: token            | +-----------------+ |
+-------------------------+ Scope: profile contacts         | | Password        | |
                                                            | +-----------------+ |
                                                            +---------------------+
                                                                      ||
                                                                      || Request consent
                                                                       V from resource owner
+-------------------------+                                 +---------------------+
|    yelp.com/callback    |                                 | accounts.google.com |
|                         |   Back to redirect URI          |                     |
|        Loading...       |<================================| Allow Yelp to access|
|                         |   with token                    | your public profile |
+-------------------------+                                 | and contacts?       |
     ^                                                      |     +-----+ +-----+ |
     ||                                                     |     | Yes | | No  | |
     || Talk to resource server                             |     +-----+ +-----+ |
     || with access token (front channel)                   +---------------------+
     V
+---------------------+
| contacts.google.com |
+---------------------+

Everything is done on the front channel.
```

### Identity Use cases (pre-2014)

- **Simple login** (OAuth 2.0)
- **Single sign-on across sites** (OAuth 2.0)
- **Mobile app login** (OAuth 2.0)
- **Delegated authorization** (OAuth 2.0)

### OAuth was never designed to be used for authentication.

- OAuth was designed for permissions/scopes. It doesn't care who you are.
- OpenID Connect DOES NOT replace OAuth 2.0.
- OpenID Connect replaces misusing OAuth for authentication.

### Problems with OAuth 2.0 for authentication

- No standard way to get the user's information.
  - When Facebook, Google, etc used OAuth 2.0 for authentication the each added their own custom hacks on top of OAuth the get the user's info and some other things that OAuth is missing.
    - Each implementation is different and there is no standard.
- Every implementation is a little different.
- No common set of scopes.

## OAuth 2.0 and OpenID Connect

- **OpenID Connect** is for authentications.
  - It's really not it's own protocol, but a small layer on top of OAuth 2.0.
- **OAuth 2.0** is for authorization.

```
+----------------+
| OpenID Connect |
+----------------+
| OAuth 2.0      |
+----------------+
| HTTP           |
+----------------+

```

### What OpenID Connect adds

- ID token (has information about the user)
- UserInfo endpoint for getting more user information
- Standard set of scopes
- Standardized implementation

_OpenID Connect authorization code flow_

```
 Resource
  Owner
    |
    V     Client                                            Authorization Server
+-------------------------+                                 +---------------------+
|        yelp.com         |                                 | accounts.google.com |
|                         | Go to authorization server      | +-----------------+ |
| +---------------------+ |================================>| | Email           | |
| | Log in with Google  | | Redirect URI: yelp.com/callback | +-----------------+ |
| +---------------------+ | Response type: code             | +-----------------+ |
+-------------------------+ Scope: openid profile           | | Password        | |
                                                       +--> | +-----------------+ |
                      Exchange authorization code for  |    +---------------------+
                   +-----------------------------------+              ||
                   |  access token and ID token                       || Request consent
                   V                                                   V from resource owner
+-------------------------+                                 +---------------------+
|    yelp.com/callback    |                                 | accounts.google.com |
|                         |   Back to redirect URI          |                     |
|        Hello Nate       |<================================| Allow Yelp to access|
|                         |   with authorization code       | your public profile |
+-------------------------+                                 | and contacts?       |
     ^                                                      |     +-----+ +-----+ |
     |                                                      |     | Yes | | No  | |
     | Get user info                                        |     +-----+ +-----+ |
     | with access token (back channel )                    +---------------------+
     V
+---------------------+
| accounts.google.com |
|     /userinfo       |
+---------------------+
```

## The ID token is a JSON web token (or JWT).

- A standard way of encoding information in a way that's easy to transmit over the internet.
- You can see what is in the web token if you copy and paste it into a decoding tool like [https://jsonwebtoken.io](https://jsonwebtoken.io).
- Anatomy of the ID token (JWT)

```
(HEADER)
.
{
    "iss": "https://accounts.google.com",
    "sub": "you@gmail.com",
    "name": "Nate Barbettini",
    "aud": "s6BhdRkqt3",
    "exp": 1311281970,
    "iat": 1311281970,
    "auth_time": 1311280969,
}
.
(Signature)
```

- The signature can be used to verify that the ID token hasn't been modified or compromised in flight.
- The ID token can be independently verified by the application without making a call to the authorization erver.

- (Signature)

### Callingthe userinfo endpoint

```
GET www.googleapis.com/oauth2/v4/userinfo
Authorization: Bearer fFAGRNJru1FTz70BzhT3Zg

200 OK
Content-Type: application/json

{
    "sub": "you@gmail.com",
    "name": "Nate Barnettini",
    "profile_picture": "http://plus.g.co/123"
}
```

### Identity Use cases (today)

- **Simple login** (OpenID Connect) - Authentication
- **Single sign-on across sites** (OpenID Connect) - Authentication
- **Mobile app login** (OpenID Connect) - Authentication
- **Delegated authorization** (OAuth 2.0) - Autorization

## OAuth and OpenID Connect

### Use OAuth 2.0 for: (Authorization)

- Granting access to you API
- Getting access to user data in other systems

### Use OpenID Connect for: (Authentication)

- Logging the user in
- Making you accounts available in other systems

### Which grant type (flow) do I user?

- **Web application w/ server backend**: authorization code flow

```
+---------------+                                     +-------------------+
|  example.com  |  OpenID Connect (code flow)         | login.example.com |
| +-----------+ |====================================>| +---------------+ |
| | Log in    | |<====================================| | Email         | |
| +-----------+ |  Back to web page with code         | +---------------+ |
+---------------+  grant, exchanged for ID token      | | Password      | |
      ||                                              | +---------------+ |
      ||                                              +-------------------+
      ||                                              Authorization server handles
      ||                                              login and security, establishes
      V                                               session for user
Set-Cookie: sessionid=f00b4r; Max-age: 86400;

The authentication system and the rest of the app are a little abstracted
so the app and the authentication system can evolve separately.
```

- **Native mobile app**: authorization code flow with PKCE

```
+---------------+                                     +-------------------+
|  example.com  |  OpenID Connect (code flow + PKCE)  | login.example.com |
| +-----------+ |====================================>| +---------------+ |
| | Log in    | |<====================================| | Email         | |
| +-----------+ |  Back to app with code grant        | +---------------+ |
+---------------+  exchanged for ID token and         | | Password      | |
      ||           access token                       | +---------------+ |
      ||                                              +-------------------+
      ||                                              Authorization server handles
      ||                                              login and security, establishes
      V                                               session for user
Stores tokens in protected device storage.
User ID token to know who the user is.
Attach access token to outgoing API requests.

PKCE is Proof Code for Key Exchange (sometimes called pixie)
AppAuth library does this all for you.
```

- **JavaScript app (SPA) w/ API backend**: implicit flow

```
+---------------+                                     +-------------------+
|  example.com  |  OpenID Connect (implicit flow)     | login.example.com |
| +-----------+ |====================================>| +---------------+ |
| | Log in    | |<====================================| | Email         | |
| +-----------+ |  Back to web page with ID token     | +---------------+ |
+---------------+  and access token                   | +---------------+ |
      ||                                              | | Password      | |
      ||                                              | +---------------+ |
      ||                                              +-------------------+
      ||                                              Authorization server handles
      V                                               login and security, establishes
Stores tokens locally with JavaScript.                session for user
User ID token to know who the user is.
Attach access token to outgoing API requests.
```

- **Microservices and APIs**: client credentials flow

### Example SSO with third-party services

```
+---------------+                     +------------+
|  example.com  |  OpenID Connect     |            |
| +-----------+ |<===================>|    Okta    |
| | Log in    | |                     |            |
| +-----------+ |                     +------------+
+---------------+                            ^
                                            || SAML
                                            ||
                                            V
                                    +----------------------+
                                    | saml.othersite.com   |
                                    | +------------------+ |
                                    | | Email            | |
                                    | +------------------+ |
                                    | +------------------+ |
                                    | | Password         | |
                                    | +------------------+ |
                                    +----------------------+
```
