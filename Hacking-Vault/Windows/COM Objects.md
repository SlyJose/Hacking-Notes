
The Microsoft Component Object Model (COM) is a platform-independent, distributed, object-oriented system for creating binary software components that can interact. 

Think of COM (Component Object Model) objects as Lego blocks for software. Each Lego block (COM object) has a specific purpose, like making a car or a spaceship. Similarly, each COM object has its own set of functions and abilities that software programs can use.

Now, imagine you're building something cool with Legos, like a castle. You don't need to know how each Lego block is made or how it works internally. You just need to know how to connect them together to build what you want. In the same way, software programs can use COM objects without needing to know exactly how they're built internally. They just need to know how to use them to get their job done.

So, COM objects are like building blocks that software programs can use to add functionality without having to reinvent the wheel every time. They make it easier for developers to build complex software by providing pre-made components that they can plug into their programs.

To understand COM (and therefore all COM-based technologies), it is crucial to understand that it is not an object-oriented language but a standard. Nor does COM specify how an application should be structured; language, structure, and implementation details are left to the application developer. Rather, COM specifies an object model and programming requirements that enable COM objects (also called COM components, or sometimes simply _objects_) to interact with other objects. These objects can be within a single process, in other processes, and can even be on remote computers. They can be written in different languages, and they may be structurally quite dissimilar, which is why COM is referred to as a _binary standard_; a standard that applies after a program has been translated to binary machine code.

The only language requirement for COM is that code is generated in a language that can create structures of pointers and, either explicitly or implicitly, call functions through pointers. Object-oriented languages such as C++ and Smalltalk provide programming mechanisms that simplify the implementation of COM objects, but languages such as C, Java, and VBScript can be used to create and use COM objects.

## 🖊️ Functions

1. **Initialization and Cleanup Functions:** These functions are used to initialize the COM object when it's created and to clean up any resources it's using when it's no longer needed.
    
2. **Property Accessors:** COM objects often have properties that you can get or set to control their behavior. For example, a COM object representing a file might have properties like "filename" or "file size" that you can read or modify.
    
3. **Methods:** These are the main functions that do the work the COM object is designed for. For example, a COM object representing a printer might have methods like "printDocument" or "cancelPrintJob" that you can call to print a document or cancel a print job.
    
4. **Event Handlers:** Some COM objects can raise events to notify other parts of the program when something happens. For example, a COM object representing a timer might raise an event when the timer expires.
    

These are just a few examples, and the functions available in a COM object will depend on what it's designed to do. But in general, COM objects provide a way for software programs to interact with pre-made components that provide specific functionality.


## 📔 Description

- 

##  📗 Action to perform 

1. 


### Properties
---
📆 created   {{07-02-2024}} 14:40
🏷️ tags: #Windows


---

