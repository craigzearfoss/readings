# OAuth 2.0 and OpenID Connect (in plain English)

- OktaDev
- Speaker: Nate Barbettini
- [https://www.youtube.com/watch?v=996OiexHze0&t=1734s](https://www.youtube.com/watch?v=996OiexHze0&t=1734s)
- Slide: [https://speakerdeck.com/nbarbettini/oauth-and-openid-connect-in-plain-english](https://speakerdeck.com/nbarbettini/oauth-and-openid-connect-in-plain-english)
- Feb 5, 2018
- Completed Apr 15, 2020

---

Buddy Holly's Greatest Hits
The Beatle Yellow Submarine
The Clash London Calling
Elvis Costello - My Aim Is True
XTC - Black Sea
Gene Vincent's Greatest Hist
Robert Cray/Albert Collins/Lonnie Brooks - Showdown
Southern Culture on the Skids - Too Much Pork
Hillbilly Frankenstein - Hypnotica
Zen Frisbee - Mad As Faust
Sleazefest
Tokyo Jihen

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

```
+---------------------------+           +-----------------------+
|         yelp.com          |           |  accounts.google.com  |
|                           |           |  +-----------------+  |
|  +---------------------+  |==========>|  | Email           |  |
|  | Connect with Google |  |           |  +-----------------+  |
|  +---------------------+  |           |  +-----------------+  |
+---------------------------+           |  | Passowrd        |  |
                                        |  +-----------------+  |
                                        +-----------------------+
                                                  |
                                                  V
+---------------------------+           +-----------------------+
|     yelp.com/callback     |           | accounts.googl.com    |
|                           |           |                       |
|         Loading...        |<==========| Allow Yelp to access  |
|                           |           | your public profile   |
+---------------------------+           | and contacts?         |
                                        |      +-----+ +-----+  |
                                        |      | Yes | | No  |  |
                                        |      +-----+ +-----+  |
                                        +-----------------------+
```

## OAuth 2.0 terminology

- **Resource owner**
- **Client**
- **Auhorization server**
- **Resource server**
- **Authorization grant**
- **Redirect URI**
- **Access Token**
