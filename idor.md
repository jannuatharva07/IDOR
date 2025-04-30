# IDOR

## Definition

IDOR stands for **Insecure Direct Object Reference**.  
It is a **subcategory of Access Control vulnerabilities**.

*PortSwigger Definition —  
IDOR arises when an application uses user-supplied input to access objects directly.*

In simple words, IDOR is a vulnerability where the application fails to enforce proper access control, allowing an **unauthorized user to access or manipulate resources** they shouldn't be able to access.

### 🧪 Example

Imagine a web application that stores user invoices:  
Ravi accesses his invoice using this URL:  
`https://example.com/invoice?id=1001`  

Now, if Sam changes the ID to `1002`:  
`https://example.com/invoice?id=1002`  

— and he can see **Ravi's invoice** without authorization, this indicates an **IDOR vulnerability**.  

Why? Because the application is **directly referencing objects (like invoice IDs)** without verifying **if the user is allowed to access them**.

## Scenarios - IDOR
### 1. Retrieve a Database Record  
   URL has something like:`https://foo.bar/somepage?invoice=12345`  
   The invoice value is used directly to fetch data from the database.  
   If you change the ID (e.g. 12346), you may get another user’s invoice → ❗ IDOR
### 2. Perform an Operation in the System  
   URL:`https://foo.bar/changepassword?user=someuser`   
   The user parameter tells the app whose password to change.  
   If the app doesn’t check who is logged in, changing the user value can let you reset someone else’s password → ❗ IDOR
### 3. Retrieve a File System Resource  
   URL:`https://foo.bar/showImage?img=img00011`  
   The img parameter directly decides which file to load.  
   If you change it to another file (e.g. img=img00012), and it works → ❗ IDOR  
   *`⚠️ May also work with Path Traversal (e.g., img=../../secret.txt)`*
### 4. Accessing Functionality via Parameter
   URL:`https://foo.bar/accessPage?menuitem=12`  
   The menuitem controls what feature/page you access.  
   If you’re only allowed menuitem=1,2,3 but access 12 anyway → ❗ IDOR  

## 🔎 Where to Find IDORs?

IDORs often hide in plain sight! Here's where to look:

### 📍 Common Spots
- Any **ID-like values** in URLs or requests (e.g., `userId`, `invoice`, `docId`)
- **APIs** are full of IDORs – especially REST APIs
- Apps with **complex permissions** or roles (admin/user/mod)
- **CRUD** operations – test each on someone else’s data:
  - **Create ➕** – Can you create something *on behalf of another user*?
  - **Read 👁️** – Can you view *someone else’s private data*?
  - **Update ✏️** – Can you modify *another user’s content*?
  - **Delete ❌** – Can you delete *someone else’s resource or account*?

## 🧠 Smart IDOR Hunting

- Always inspect the **ID structure** (plain, base64, encrypted?).
- Look for **leaks** (e.g., public profile showing a user ID).
- Spot **patterns** in IDs (incrementing, UUID, usernames,).
- Try **generating IDs** yourself or use **Burp Intruder** to fuzz.

## Base Steps to Test for IDOR

1. **Create 2 accounts** (e.g., userA and userB).

2. **Find endpoints** and check:
   - Does the URL or request contain any **ID or username**?
   - Is the **endpoint public** (no login needed) or **private** (requires login)?

3. While logged in as userA, **change the ID/username in the request to userB's** and check:
   - Can you view, update, delete, or perform actions on userB’s data?

4. If yes → 🎯 **Potential IDOR found!**

## 🔍 IDOR Test Cases

### Testcase - 1: Add IDs to requests that don’t have them

```jsx
GET /api/MyPictureList → /api/MyPictureList?user_id=<other_user_id>

Pro tip: You can find parameter names to try by deleting or editing other objects and seeing the parameter names used.
```

### Testcase - 2: Try replacing parameter names

```jsx
Instead of this:
GET /api/albums?album_id=<album id>

Try This:
GET /api/albums?account_id=<account id>

Tip: There is a Burp extension called Paramalyzer which will help with this by remembering all the parameters you have passed to a host.
```

### Testcase - 3: Supply multiple values for the same parameter.

```jsx
Instead of this:
GET /api/account?id=<your account id> →

Try this:    
GET /api/account?id=<your account id>&id=<admin's account id>

Tip: This is known as HTTP parameter pollution. Something like this might get you access to the admin’s account
```

### Testcase - 4: Try changing the HTTP request method when testing for IDORs

```jsx
Instead of this:
POST /api/account?id=<your account id> →

Try this:    
PUT /api/account?id=<your account id>

Tip: Try switching POST and PUT and see if you can upload something to another user’s profile. For RESTful services, try changing GET to POST/PUT/DELETE to discover create/update/delete actions.
```

### Testcase - 5: Try changing the request’s content type


Instead of this:

```jsx
POST /api/chat/join/123
[…]
Content-type: application/xml → 
<user>test</user>    
```
Try this:
```jsx
POST /api/chat/join/123
[…]
Content-type: application/json
{“user”: “test”}
```

Tip: Access controls may be inconsistently implemented across different content types. Don’t forget to try alternative and less common values like text/xml, text/x-json, and similar.
```

### Testcase - 6: Try changing the requested file type (Test if Ruby)

```jsx
Example:

GET /user_data/2341 --> 401 Unauthorized

GET /user_data/2341.json --> 200 OK

Tip: Experiment by appending different file extensions (e.g. .json, .xml, .config) to the end of requests that reference a document.
```

### Testcase - 7: Does the app ask for non-numeric IDs? Use numeric IDs instead

```jsx
There may be multiple ways of referencing objects in the database and the application only has access controls on one. 
Try numeric IDs anywhere non-numeric IDs are accepted:
Example:

username=user1 → username=1234
account_id=7541A92F-0101-4D1E-BBB0-EB5032FE1686 → account_id=5678
album_id=MyPictures → album_id=12
```

### Testcase - 8: Try using an array

```markdown
If a regular ID replacement isn’t working, try wrapping the ID in an array and see if that does the trick. For example:

{“id”:19} → {“id”:[19]}
```

## Testcase - 9: Wildcard ID

```markdown
These can be very exciting bugs to find in the wild and are so simple. Try replacing an ID with a wildcard. You might get lucky!

GET /api/users/<user_id>/ → GET /api/users/*
```

### Testcase - 10: Pay attention to new features

```markdown
If you stumble upon a newly added feature within the web app, such as the ability to upload a profile picture for an upcoming charity event, and it performs an API call to:

/api/CharityEventFeb2021/user/pp/<ID>

It is possible that the application may not enforce access control for this new feature as strictly as it does for core features.
```
