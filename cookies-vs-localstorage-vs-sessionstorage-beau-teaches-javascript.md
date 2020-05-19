# cookies vs localStorage vs sessionStorage - Beau teaches JavaScript

- freeCodeCamp.org
- [cookies vs localStorage vs sessionStorage - Beau teaches JavaScript](https://www.youtube.com/watch?v=AwicscsvGLg)
- [FUN PHP - Higher order functions](https://www.youtube.com/watch?v=sUJ8j2UR5kU)
- May 16, 2017
- Completed May 19, 2020

---

```
                    Cookies           Local Storage     Session Storage
                    ----------------  ----------------  ---------------
Capacity            1kb               10mb              5mb
Browsers            HTML4/HTML5       HTML5             HTML5
Accessible from     Any window        Any window        Same tab
Expires             Manually set      Never             On tab close
Storage Location    Browser & Server  Browser only      Browser only
Sent with Requests  Yes               No                No
```

### Cookies
- Sent to the server on every request.
- If you do not set an expiration date then by default they expire when the session ends.
- The **path** specifies which path on your website you want to associate the cookie with.
- You cannot get one cookie at a time. (They get stored in one string.)
- To delete a cookie you set an expired expiration date.
```
// set an item
document.cookie = "hello=true";

// set an item with an expiration date
document.cookie = "doSomethingOnlyOnce=true", "expires=Fri, Dec 31 9999 23:59:59 GMT";

// set an item with an expiration date and a path
document.cookie = "person=beau", "expires=Fri,Dec 31 9999 23:59:59 GMT; path=/";

// get the cookies
console.log(document.cookie);

// delete a cookie
document.cookie = "person=", "expires=Thu, 01 Jan 1970 00:00:00 UTC; path=/";
```

### localStorage
```
// set an item
localStorage.setItem("breakfast", "cereal");

// retrieve an item
console.log(getStorage.setItem("breakfast"));

// remove an item
localStorage.removeItem("breakfast");

// clear all items
localStorage.clear();
```

### sessionStorage
```
// set an item
sessionStorage.setItem("breakfast", "cereal");

// retrieve an item
console.log(sessionStorage.setItem("breakfast"));

// remove an item
sessionStorage.removeItem("breakfast");

// clear all items
sessionStorage.clear();
```