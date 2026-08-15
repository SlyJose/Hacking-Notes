
A [[Trust Relationship]] between domains allows you to authorize a user from domain A to access resources from domain B.

# 📔 Types of trust relationships

- One-way trust relationship: In a one-way trust, if Domain AAA trusts Domain BBB, this means that a user on BBB can be authorized to access resources on AAA. In one-way trusts can be labelled as **Inbound** or **Outbound** depending on your perspective. For this example Domain AAA has an outbound trust and Domain BBB has an Inbound trust
- Two-way trust relationships: Allow both domains to mutually authorise users from the other. By default, joining several domains under a tree or a forest will form a two-way trust relationship.

##### Transitivity

Transitivity defines whether or not a trust can be chained. For instance - if Domain A trusts Domain B, and Domain B trusts Domain C; then A also trust C. If these domains are owned by different entities, then there are obvious implications in terms of the trust model…

It is important to note that having a trust relationship between domains doesn't automatically grant access to all resources on other domains. Once a trust relationship is established, you have the chance to authorize users across different domains, but it's up to you what is actually authorized or not.

#### 🖊️ How it works

A trust relationship enables users in one domain to authenticate and access resources in another domain. This works by allowing authentication traffic to flow between them using referrals. When a user requests access to a resource outside of their current domain, their KDC will return a referral ticket pointing to the KDC of the target domain. 

The user's TGT is encrypted using an inter-realm trust key (rather than the local krbtgt), which is often called an inter-realm TGT. The foreign domain decrypts this ticket, recovers the user's TGT and decides whether they should be granted access to the resource or not.

#### 📗 Security Risks

[[Exploit Trust Relationships]]
[[Exploit One-Way Inbound]]
[[Exploit One-Way Outbound]]

### Properties
---
📆 created   {{22-10-2023}} 22:30
🏷️ tags: #activedirectory   #crto 

---

