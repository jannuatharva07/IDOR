**Learn IDOR from hacktivity reports**

# Hacktivity Reports

## 1. **IDOR - Accessing Payment Data of Other Users**
**Report ID:** 751577  
**What Happened:**  
An attacker sent a request to the server with a `userid` parameter. They changed the `userid` and were able to see the payment details of other users.  
**Lesson Learned:**  
Test APIs to make sure that changing the user ID in the URL or request doesn’t let someone see or access other users' private data. This is called **IDOR (Insecure Direct Object Reference)**.  
**[Read Full Report](https://hackerone.com/reports/751577)**

---

### 2. **IDOR - Deleting Other User's Certifications**
**Report ID:** 2122671  
**What Happened:**  
The attacker was logged into two accounts (Account A and Account B) at the same time. They created certifications in both accounts. By using a burp, they changed the ID in the request to delete certifications from another user’s account.  
**Lesson Learned:**  
Make sure users can only delete or edit their own data. Don’t allow one user to delete or modify another user's information.  
**[Read Full Report](https://hackerone.com/reports/2122671)**

---

### 3. **IDOR - Deleting Campaigns**
**Report ID:** 1969141  
**What Happened:**  
The attacker found that the IDs of campaigns were not hidden. By changing the campaign ID in the URL, the attacker was able to delete someone else's campaign.  
**Lesson Learned:**  
Always hide or encrypt IDs in requests. If users can easily change IDs in URLs, they might be able to perform actions they shouldn't, like deleting data from other users.  
**[Read Full Report](https://hackerone.com/reports/1969141)**

---

### 4. **IDOR - Missing Ownership Check on Remote Wipe**
**Report ID:** 319807  
**What Happened:**  
There was a feature to remotely wipe a user’s device using this URL:
/settings/p/auth/wipe/{device_id}

But the server didn’t check if the device belonged to the person making the request.  
By changing `{device_id}` to another user’s ID, an attacker could wipe someone else’s device.  
**Lesson:** Always verify **ownership** before performing sensitive operations.  
**[Read Full Report](https://example.com/report/319807)**

---

### 5. **IDOR - Edit Anyone's Blog/Website Info**
**Report ID:** 974222  
**What Happened:**  
1. Attacker and victim had accounts on a website.  
2. Victim added a blog/website in their profile settings.  
3. Attacker viewed source code and noted the victim’s `id`.  
4. Attacker went to their own profile, intercepted the blog/website update request using Burp Suite.  
5. They changed the `id` to the victim’s and were able to change victim's blog info.

**Lesson:** Never allow users to update other users’ data just by changing an ID.  
**[Read Full Report](https://example.com/report/974222)**

---

### 6. **IDOR - Account Takeover via Edit User (Crowd Signal)**
**Report ID:** 915114  
**What Happened:**  
There was an endpoint that let users edit their own account info.  
By modifying the `id` in the request URL, the attacker accessed and edited other users’ data.  
**No user interaction was needed** — attacker didn’t need any help from the victim.  
**Lesson:** Never trust just the `id` in a request — **always verify the user behind the ID.**  
**[Read Full Report](https://example.com/report/915114)**

---

### 7. **IDOR - Change Product Price During Checkout**
**Report ID:** 1403176  
**What Happened:**  
Attacker changed the price of a product while buying it!  
**Steps to Reproduce:**
1. Go to product page on `xyz.com`, add item to cart.
2. Fill in details, go to checkout.
3. Intercept the request using **Burp Suite**.
4. Modify the `price` parameter in the request before it reaches the server.
5. Forward the request — the product was purchased at the changed (lower) price.

**Lesson:** Never trust prices from the client side — always calculate final price on server side.  
**[Read Full Report](https://example.com/report/1403176)**

---

### 8️⃣ 1213237 – IDOR by Changing Method to DELETE  
Just changed **GET** to **DELETE** on message endpoint with `msg_id` in URL  
➡️ Deleted someone else’s message  


### 9️⃣ 703894 – JSON Reveals Raw User Data  
Some endpoints like `.../started.json` expose user data in raw JSON format  
➡️ No authentication or filtering → Sensitive data exposed

---

### 🔟 888729 – Read-Only User Can Delete Users  
ID was in URL.  
Two orgs created (H1 and H2) with guest users.  
➡️ In H2, admin deletes guest → Intercept request  
➡️ Change ID to guest from H1 → That guest gets deleted 😬  
❗ No proper permission check → Classic IDOR

---

### 🔟 1272478 – ATO (Account Takeover) via IDOR  
1️⃣ Create 2 accounts – Attacker & Victim  
2️⃣ Attacker updates their profile → Capture request (contains user ID)  
3️⃣ Find victim’s user ID  
4️⃣ Replace attacker’s ID with victim’s in the request  
5️⃣ Change username → Now victim account is linked to attacker’s username  
6️⃣ Reset password using that username → 🔓 Logged in as victim 😱  
❗ No proper ownership check → ATO via IDOR

---
### 1️⃣1️⃣ 1819832 – Snapchat: Delete Anyone’s Spotlight Content 📸  
1️⃣ Attacker captures their own spotlight **delete request**  
2️⃣ ID of the content is in the request URL  
3️⃣ Attacker finds another post’s ID (visible in spotlight URLs)  
4️⃣ Changes the ID in the request → Deletes someone else’s post ❌  
❗ No ownership check on delete request = IDOR!
