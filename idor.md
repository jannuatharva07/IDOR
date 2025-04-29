# IDOR

## Definition

IDOR stands for **Insecure Direct Object Reference**.  
It is a **subcategory of Access Control vulnerabilities**.

*PortSwigger Definition —  
IDOR arises when an application uses user-supplied input to access objects directly.*

In simple words, IDOR is a vulnerability where the application fails to enforce proper access control, allowing an **unauthorized user to access or manipulate resources** that they shouldn't have access to.

### 🧪 Example

Imagine a web application that stores user invoices:  
Ravi access his invoice using this URL:  
`https://example.com/invoice?id=1001`  

Now, if Sam change the ID to `2002`:  
`https://example.com/invoice?id=2002`  
— and he can see **Ravi's invoice** without being authorized,  
this indicates an **IDOR vulnerability**.

Why? Because the application is **directly referencing objects (like invoice IDs)** without checking **if you’re allowed to access them**.
