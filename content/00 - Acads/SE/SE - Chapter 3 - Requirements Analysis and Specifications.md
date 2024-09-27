---
time created: 2024-09-26 23:10
tags:
  - academics
  - software-engineering
references:
  - "[[Exam Schedules and Syllabus]]"
follows:
---
---
# Overview
**Requirement**: A capability or condition required from the system. 

> **What is involved in requirement analysis and specification (RAS)?**
> We determine what the client is expecting and we document them in a form to make it clear to the client and the dev team. 

[[se-requirements-engineering-process-flowchart.canvas|se-requirements-engineering-process-flowchart]]

> [!faq]- Why do we need an SRS?
> Requirements errors are very expensive to fix, and requirement changes cost a lot. A good SRS avoids both of these, thus greatly reducing development costs. 
> 
> An SRS provides a starting point for general development. It provides the basis for estimating costs, schedules etc. It also forms the basics for writing the user manual, which is a very important document for the users. 

> The SRS is mainly intended for users/customers, developers, testers, project managers and system analysts. Different levels of detail and formality is needed for each audience.

## How To Gather Requirements

There are various ways to gather requirements. Some are listed below: 
1. Discuss them with customers. 
2. Study existing procedures. 
3. Analyse what needs to be done. Includes:
	1. Scenario Analysis -> Various scenarios that might arise
	2. Task Analysis
	3. Form Analysis -> A collection of different types of forms being used.

> The attributes of a good system analyst are **experience**, and **good interaction skills**. 
## Analysis Of Gathered Requirements
The main reasons for gathering requirements is to:
- Clearly understand the users' requirements. 
- Detect inconsistencies, such as having some part of the requirements contradict another part. 
- Helps the system analyst understand what the problem is and why is it important to solve it. 

---
# SRS - Software Requirements Specification
The **main aim** of the [[Software Requirements Specification (SRS)]] is to document and systematically organise the arrived requirements. 

> The SRS document is often referred to as a **Black Box Specification**. (The internal details are not known, but the external details are documented). 

---
## Properties Of A Good SRS
1. It should be **concise**.
2. It should be **consistent**. 
3. It should be **flexible/easy-to-change**.
4. It should be **complete**. 

> The SRS should **NOT** include the:
> - Design
> - Project Development Plans
> - Product Assurance Plan

The SRS lasts as long as the software isn't made obsolete.

---

## Most Important Parts of the SRS
The following are the most important parts of the SRS:
1. [[#Functional Requirements]] -> The heart of the SRS, they form the bulk of the document.
2. [[#External Interfaces]]
3. [[#Constraints]]
4. [[#Non-Functional Requirements]]
### Functional Requirements
Functional requirements specify the expected behavior and oeprations of a system. These are the core capabilities that the system must perform to meet the user's needs. Functional requirements focus on *what* the system must do and *how* it will interact with users, data, and other systems. 

The following are the key components of functional requirements:
- **I/O Behavior**: Specifies the data that teh system should accept and what it will produce in return. Example -> Allowing users to enter login credentials and return access to the dashboard upon successful authentication.
- **Processes or Functions**: Descibes the logical steps or procedures that the system must follow to process inputs and generate outputs. Example -> The system must validate the user's credentials by checking them against a database before granting access. 
- **Error Handling**: Specifies how the system should handle incorrect inputs or unexpected conditions. 
- **Behavior for Different Scenarios**: Specifies the system's responses to various scenarios and conditions that may arise during its use. Example -> The system must lock the user out after five unsuccessful login attempts and provide a password recovery option. 
- **Interaction with External Systems**: Details how the system interfaces and exchanges data with other systems or third-party services. Example -> Razorpay integration for payment handling .
- **Data Management**: Outlines how the system will handle the storage, retrieval and modification of the data. Example -> The system must allow users to update their personal details and save changes in the central database.
### External Interfaces
Consists of the following:
- User Interfaces
- Hardware Interaces
- Software Interfaces
### Constraints
These consists of the hardware and the OS to be used.
### Non-Functional Requirements
They consist of the characteristics that **cannot be expressed as functions**. These include:
1. Portability
2. Usability
3. Security
4. Safety
5. Maintainability

---
## IEEE Standards For Writing SRS
1. Title
2. Table of Contents
3. Introduction (PSDRO)
	1. Purpose
	2. Scope
	3. Definition
	4. References
	5. Objectives
4. Overall Description
5. Specific Requirements
6. Appendices
7. Index
---

> [!info] What is **Partitioning**?
> Decomposing a problem and establishing a hierarchical representation of the problem is called *partitioning*. 

---
## Examples Of A Bad SRS Document
The following are some features of a bad SRS document:
- **Contradictions** -> The same thing being described in different ways.
- **Ambiguity** 
- **Noise** -> Irrelevant text.
- **Silence** -> Omission of actually relevant text.

***
## Suggestions For Writing Quality Requirements
- Keep sentences and paragraphs short.
- Use the active voice along with proper grammar and spelling.
- Split a requirement into sub-requirements.
- Read requirements from a developer's perspective.

> [!faq]- What is the difference between **System Specification** and **Software Specification**? 
> The system specification concerns the full functionality of the system, hardware **and** software included, whereas software specification is, obviously, focused on just teh software part. 

---
## Representation of Complex Processing Logic
We use **Decision Trees** and **Decision Tables** to handle the representation of complex processing logic. 

### **Decision Trees**
For the case of **Decision Trees**, the edges represent *conditions* and the leaf nodes present *actions that need to be performed*. 

### **Decision Tables**
A **Decision Table** is a structured, tabular method for the representation of complex processing logic, conditions and the corresponding actions that need to be taken based on those conditions. It is particularly useful in situations where multiple conditions need to be evaluated simultaneously, providing a clear, concise way to model decision-making processes.

A typical decision table is divided into four parts:
1. **Conditions**: These represent the various inputs or factors that influence the decision-making process. Example -> Is the customer a Pro member? Has the payment been processed?
2. **Actions**: These are outcomes or decisions that result from evaluating the conditions. Example -> Grant access to Pro features, or Decline Transaction.
3. **Rules**: Each column in the table represents a **rule**. Each rule determines the resulting action from that particular set of conditions. Example -> If a customer is a Pro member and the payment has been processed, grant access to pro features.
4. **Condition Entries and Action Entries**: Condition entries are the values that can be **true (Y)**, or **false (N)** or **don't care (-)** for each condition. Action entries are the actions corresponding to each rule, indicating whether the action is performed or not. 

> In the upper rows of the decision table, the conditions are to be evaluated. In the lower rows, actions are to be taken when the conditions are satisfied. The column represents a rule. 


> [!faq]- What is the main difference between Decision Trees and Decision Tables?
> Decision trees are easier to read and understand when the number of conditions **are small**. 


---
