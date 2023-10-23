
[[Active Directory]] supports integrating multiple [[Windows Domain]]s so that you can partition your network into units that can be managed independently. If you have two domains that share the same namespace, those domains can be joined into a [[Tree]].

This partitioned structure gives us better control over who can access what in the domain. 

![[Tree.png]]

In the previous example, the IT people from the UK will have their own DC that manages the UK resources only. A UK user would not be able to manage US users. In that way, the Domain Administrators of each branch will have complete control over their respective DCs, but not other branches' DCs. Policies can also be configured independently for each domain in the tree.





### Properties
---
📆 created   {{22-10-2023}} 21:40
🏷️ tags: #activedirectory 
---

