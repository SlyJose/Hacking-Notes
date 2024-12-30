
#### 🚀 - Long Term access
---
Programmatic Access ( Access Key ID + Access Key Secret ):

`aws configure --profile atomic-nuclear`

Profiles are usually used when you have multiple credentials. 

---
#### 📦 - Short Term access

Programmatic Access ( Access Key ID + Access Key Secret + Session Token )

Go to your ~/.aws folder and open the "credentials" file, write down the credentials like this:
```
[default]
[new-profile-name]
aws_access_key_id = ASIAUI7PQBNFXFGHTQ67
aws_secret_access_key = Bps0OxO8w1kLAKRS/ECvbYsAipQkIQzsPmqQ6gaU
aws_session_token = IQoJb3JpZ2luX2[...]
```

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
