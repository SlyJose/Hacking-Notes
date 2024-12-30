
AWS current architecture is structured as follows:

![[aws-architecture.png]]

#### 🖊️ Cloud  Space

Composed of three areas:
- GUI: Web portal GUI to get into the portal
- Control Plane: Any action in the GUI is handled by the control plane which is composed of AWS services
- Data Plane: any resources created by the control plane, is created in the data plane.

#### 📔 End User

Involves the ways the user interacts with the cloud space. If you want to use the web client you need IAM or SSO creds. If you want to interact with control plane you need either AWS CLI or SDK API that use long and short term keys.


####  📗 C


#### ⚠ Opsec




### Properties
---
📆 created   {{15-12-2024}} 10:52
🏷️ tags: #cloud #aws 

---

