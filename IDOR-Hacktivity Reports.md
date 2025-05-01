> 🔰 **New to IDOR?**  
> Start here first: [Introduction to IDOR](https://github.com/jannuatharva07/IDOR/blob/main/idor.md)    
---

**Learn IDOR from hacktivity reports**

# Hacktivity Reports

## 1. **IDOR - Accessing Payment Data of Other Users**
**Report ID:** 751577  
**What Happened:**  
An attacker sent a request to the server with a `userid` parameter. They changed the `userid` and were able to see the payment details of other users.  

**Key Insight:**  
Always test for IDOR vulnerabilities in APIs by verifying if any parameters (like `user-id`) can be modified to access unauthorized data.  

**[Read Full Report](https://hackerone.com/reports/751577)**

---

## 2. **IDOR - Deleting Other User's Certifications**
**Report ID:** 2122671  
**What Happened:**  
The attacker was logged into two accounts (Account A and Account B) at the same time. They created certifications in both accounts. By using a burp, they changed the ID in the request to delete certifications from another user’s account.  

**Key Insight:**  
Always verify that the IDs used in requests (such as for certifications or licenses) are properly protected, and ensure that the correct user’s ID is validated before performing sensitive actions like deletion.  

**[Read Full Report](https://hackerone.com/reports/2122671)**

---

## 3. **IDOR - Deleting Campaigns**
**Report ID:** 1969141  
**What Happened:**  
The attacker found that the IDs of campaigns were not hidden. By changing the campaign ID in the URL, the attacker was able to delete someone else's campaign.  

**Key Insight:**  
Always check if any ID is encrypted or encoded in requests. If not, they are vulnerable to manipulation, allowing attackers to perform unauthorized actions (e.g., deleting campaigns).   

**[Read Full Report](https://hackerone.com/reports/1969141)**

---

## 4. **IDOR - Missing Ownership Check on Remote Wipe**
**Report ID:** 819807  
**What Happened:**  
There was a feature to remotely wipe a user’s device using this URL:
/settings/p/auth/wipe/{device_id}
But the server didn’t check if the device belonged to the person making the request.  
By changing `{device_id}` to another user’s ID, an attacker could wipe someone else’s device.  

**[Read Full Report](https://hackerone.com/reports/819807)**

---

## 5. **IDOR - Edit Anyone's Blog/Website Info**
**Report ID:** 974222  
**What Happened:**  
1. Attacker and victim had accounts on a website.  
2. Victim added a blog/website in their profile settings.  
3. Attacker viewed source code and noted the victim’s `id`.  
4. Attacker went to their own profile, intercepted the blog/website update request using Burp Suite.  
5. They changed the `id` to the victim’s and were able to change victim's blog info.

**Key Insight:**  
Always test if IDs in requests can be modified. Check if changing IDs in user-related data (e.g., blog, website) allows you to access or alter other users’ information.
 
**[Read Full Report](https://hackerone.com/reports/974222)**

---

## 6. **IDOR - Account Takeover via Edit User (Crowd Signal)**
**Report ID:** 915114  
**What Happened:**  
There was an endpoint that let users edit their own account info.  
By modifying the `id` in the request URL, the attacker accessed and edited other users’ data.  
**No user interaction was needed** — attacker didn’t need any help from the victim.  

**Key Insight:**  
Always check if user-specific actions can be bypassed by simply modifying the `id`.  

**[Read Full Report](https://hackerone.com/reports/915114)**

---

## 7. **IDOR - Change Product Price During Checkout**
**Report ID:** 1403176  
**What Happened:**  
Attacker changed the price of a product while buying it!  
**Steps to Reproduce:**
1. Go to product page on `xyz.com`, add item to cart.
2. Fill in details, go to checkout.
3. Intercept the request using **Burp Suite**.
4. Modify the `price` parameter in the request before it reaches the server.
5. Forward the request — the product was purchased at the changed (lower) price.

**[Read Full Report](https://hackerone.com/reports/1403176)**

---

## 8. **IDOR - Message Deletion via HTTP Method Manipulation**
**Report ID:** 1213237  
**What Happened:**  
The attacker was able to delete a message by simply changing the HTTP method.
**Steps to Reproduce:**
1. The attacker intercepted a `GET` request that displayed a message.
2. They changed the request method from `GET` to `DELETE`.
3. Added the `msg_id` in the URL path.
4. Sent the modified request — the message got deleted.

**Key Insight:**  
Sometimes, **changing the HTTP method** (e.g., `GET` to `DELETE`) can lead to unauthorized actions if the backend does not properly restrict allowed methods. Always test all common HTTP methods on endpoints and observe server behavior.

**[Read Full Report](https://hackerone.com/reports/1213237)**

---

## 9. **IDOR - JSON Endpoint Reveals Raw User Data**
**Report ID:** 703894  
**What Happened:**  
Some endpoints (like `.../started.json`) exposed raw user data in JSON format.  

**Key Insight:**  
Always look for `.json` or `.xml` versions of public pages — these can sometimes leak more information than intended.  

**[Read Full Report](https://hackerone.com/reports/703894)**

---

## 10. **IDOR - Read-Only User Can Delete Users**
**Report ID:** 888729  
**What Happened:**  
Two organizations were created: H1 and H2.  
Each had guest users.   
In org H2, the admin deleted a guest user. The request was intercepted.   
By changing the `user_id` in the request to a guest from org H1, the attacker was able to delete that user too.  

**[Read Full Report](https://hackerone.com/reports/888729)**

---

## 11. **IDOR - Account Takeover via Profile Update**
**Report ID:** 1272478  
**What Happened:**  
1. Two accounts were created: Attacker and Victim.  
2. Attacker updated their own profile and captured the request (which included their `user_id`).  
3. They found the Victim's user ID.  
4. Replaced their own ID with the Victim's in the request.  
5. Changed the username field — now the Victim's account had the attacker's username.  
6. Used "forgot password" with that username and reset the password — attacker logged in as Victim! 
  
**[Read Full Report](https://hackerone.com/reports/1272478)**

---
## 12. **IDOR - Delete Anyone’s Spotlight Content (Snapchat)**
**Report ID:** 1819832  
**What Happened:**  
1. Attacker captured the request to delete their own Spotlight post.  
2. The post ID was present in the URL.  
3. They found another user's Spotlight post ID (visible in URLs).  
4. Replaced their own ID with the victim’s post ID.  
5. Sent the request — the victim’s post got deleted!

**Key Insight:**  
If delete operations use predictable IDs without ownership checks, attackers can delete other users’ content.   

**[Read Full Report](https://hackerone.com/reports/1819832)**

---

# Reference:

[KathanP19/HowToHunt](https://github.com/KathanP19/HowToHunt/blob/master/IDOR/IDOR.md)

[Learn about Insecure Object Reference (IDOR) | BugBountyHunter.com](https://www.bugbountyhunter.com/vulnerability/?type=idor)

[Hacktivity Reports](https://hackerone.com/hacktivity)

[PortSwigger | Web Security Academy](https://portswigger.net/web-security/access-control/idor)

[WSTG - v4.2](https://owasp.org/www-project-web-security-testing-guide/v42/4-Web_Application_Security_Testing/05-Authorization_Testing/04-Testing_for_Insecure_Direct_Object_References.html)

[What I learnt from reading 220* IDOR bug reports.](https://medium.com/@Sm9l/what-i-learnt-from-reading-220-idor-bug-reports-6efbea44db7)

[Medium](https://medium.com/)

[Pentester Land](https://pentester.land/writeups/)
