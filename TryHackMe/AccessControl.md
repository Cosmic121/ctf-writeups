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
