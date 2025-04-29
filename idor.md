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
---
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
