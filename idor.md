# IDOR

## Definition

IDOR stands for **Insecure Direct Object Reference**.  
It is a **subcategory of Access Control vulnerabilities**.

*PortSwigger Definition —  
IDOR arises when an application uses user-supplied input to access objects directly.*

In simple words, IDOR is a vulnerability where the application fails to enforce proper access control, allowing an **unauthorized user to access or manipulate resources** they shouldn't be able to access.

---

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
1. Database Record via Parameter
URL has something like:
https://foo.bar/somepage?invoice=12345

The invoice value is used directly to fetch data from the database.

If you change the ID (e.g. 12346), you may get another user’s invoice → ❗ IDOR

