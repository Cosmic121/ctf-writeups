## Root cause: hiding UI instead of restricting server-side access

### Lab: Unprotected admin functionality
**What the app assumed:** 
If there's no visible link to the administrator_panel, the users wouldn't find it.

**How I found it:** 
Robots.txt is used by developers to tell website crawlers the websites that doesn't needs to be indexed. However, robots.txt was publicly available which in turn exposed "administrator_panel".

**The requests that mattered:** 
GET /robots.txt → revealed the existence of Administrator-panel
GET /administrator-panel → Grants access to admin privileges to a user

**What a proper fix looks like:** 
Setting up an login page for authentication instead of granting admin privileges by default.

**Real world impact:** 
An Attacker can easily perform Vertical Privilege Escalation and exploit admin privileges.

---

### Lab: Unprotected admin functionality with unpredictable URL

**What the app assumed:**
If the path to admin panel isn't visible on the user interface the users wouldn't find it.

**How I found it:**
Inspected the source code of the app, to find out client-side exposure inside JS gives away the path to admin panel.

**The requests that mattered:**
GET /admin-neql3z -> Grants access to the admin panel

**What a proper fix looks like:**
Setting up authorization for the admin panel 
Admin path shouldn't exist inside the client-side code of JavaScript.

**Real world impact:**
An Attacker can easily perform Vertical Privilege Escalation and exploit admin privileges.

---

### Lab: User role can be modified in user profile

**What the app assumed:**
The user would only submit the existing fields and server didn't need to validate the information

**How I found it:**
i logged in with the credentials provided via the /login path. Submitted a form which changes the user's email. intercepted the request and response with burp, which revealed the roleid field. Using repeater to modify the request which was accepted by the server without validating it, gained access to admin.

**The requests that mattered:**
POST /login -> Logged in using the credentials 
POST /my-account/change-email -> Response revealed the roleid field
POST /my-account/change-email -> Modified the JSON document my adding a roleid field with value 2
GET /admin -> Admin access gained

**What a proper fix looks like:**
The Server should validate request from client-side and should only consider existing fields of a request.

**Real world impact:**
An attacker can easily manipulate the request which would be processed by the server. This leads to exposing sensitive information and privilege escalation

