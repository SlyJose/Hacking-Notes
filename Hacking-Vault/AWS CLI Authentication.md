
#### 🚀 - Long Term access
---
Programmatic Access ( Access Key ID + Access Key Secret ):

`aws configure --profile atomic-nuclear`

Profiles are usually used when you have multiple credentials. 

---
#### 📦 - Short Term access

Programmatic Access ( Access Key ID + Access Key Secret + Session Token )

`aws configure`

You will be guided into entering all the required credentials.

--- 

#### 🖊️ - Other identity commands:

- Get information about the user's identity:
`aws sts get-caller-identity --profile [name of profile]`


#### ❗Stored credentials

AWS CLI stores credentials in the user's folder, inside a .aws hidden folder.

**Windows:** `C:\Users\username\.aws`
**Linux:** `/home/username/.aws`


### Properties
---
📆 created   {{15-12-2024}} 12:07
🏷️ tags: #cloud #aws 

---
