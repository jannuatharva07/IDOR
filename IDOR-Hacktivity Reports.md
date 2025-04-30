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
The attacker was logged into two accounts (Account A and Account B) at the same time. They created certifications in both accounts. By using a tool, they changed the ID in the request to delete certifications from another user’s account.  
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
