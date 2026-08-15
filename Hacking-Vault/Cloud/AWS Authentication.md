

#### 🖊️ Cloud Authentication Methods

![[aws-authentication.png]]

- Long Term: they never expire unless explicit
- Short Term: access tokens that are valid for a shorter periods of time. Their time is usually default.


#### 📔 AWS Authentication to the AWS Management Portal

The ways to auth in the web portal are:

➤ **IAM Root User’s** credential [Username + Password] - Long Term Access
➤ **IAM User’s** credential [Username + Password] - Long Term Access
➤ **SSO User’s** credential [Username + Password] - Long Term Access. These are usually tied to other services like Active Directory.

####  📗 AWS Authentication to AWS CLI

➤ **Long Term** : Access Key ID + Access Key Secret
➤ **Short Term** : Access Key ID + Access Key Secret + Session Token

Check [[AWS CLI]] for authentication commands.



### Properties
---
📆 created   {{15-12-2024}} 11:46
🏷️ tags: #cloud #aws 

---

