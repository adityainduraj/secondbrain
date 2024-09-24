---
time created: 2024-08-25 20:56
author: Dr. Ajeet
tags:
  - academics
references:
  - "[[Exam Schedules and Syllabus]]"
source:
---
 ## Software Development Life Cycle (SDLC)

The software development phase can be broadly classified into three phases: 
1. **Definition Phase:** Concept, Planning and Requirement Analysis
2. **Development Phase:** Design, Coding and Testing
3. **Maintenance Phase:** Implementation and Maintenance

There are many software process moels such as **Waterfall, Porotype and Spiral** models. 

## Software Process Models
1. [[#Exploratory Model]] (build and fix programming)
2. Waterfall models and its variations: 
	1. [[#Classical Waterfall Model]]
	2. Iterative Waterfall Model
	3. V Model
	4. Prototyping Model
	5. Incremental Development Model
	6. Evolutionary Model

---
3. Rapid Development Model
4. Agile Development Model
5. Spiral Model

### Exploratory Model
This is an ad-hoc approach with a two-phase model. The first phase is to write the code and the next phase is to fix it.  
Fixing means to correct or add some more functions. 
In this model, the product is developed without any proper specification or design. The product delivery time is very less. 


### Classical Waterfall Model
This is the simplest, oldest and widely used model which divides the life cycle into systematic sequential steps such as: 
1. Feasibility Study
2. Requirements Analysis and Specification
3. Design
4. Coding and Unit Testing
5. Testing (Integration and System Testing)
6. Maintenance

The phases between requirement specification and testing are called the **development phases**. 
Among all the development phases, **the testing phase consumes the maximum effort**. 
Among all the life cycle phases, **the maintenance phase consumes the maximum effort**. 

#### Feasibility Study
The main aim of the feasibility study is to determine whether developing the product is worthwhile in terms of cost, technology, operation etc. 
There are four major areas to perform a feasibilty study:
1. Economic Feasibility (Cost/Benefit Analysis)
2. Technical Feasibility (Technical Reasons)
3. Schedule Feasibility (Resource Constraints)
4. Operation Feasibility (Combination of all the above)

### Requirement Analysis and Specification
The aim of this phase is to understand the exact requirements of the customer, and then to document them properly. 
This phase consists of two distinct activities: 
1. Requirements Analysis
2. Requirements Specification

#### Goals of Requirements Analysis:
1. Collect all related data from the customer
2. Analyse the collected data to clearly understand what the customer wants
3. Find out any inconsistencies and incompleteness in the requirements
4. Resolve all inconsistencies and incompleteness

#### Goals of Requirements Specification:
The aim of the Requirements Specification is the produce a [[Software Requirements Specification (SRS)]] document after analysing the collected data and removing noise, ambiguities and contradictions. 
A good [[Software Requirements Specification (SRS)]] is correct, complete, consistent, unambiguous, traceable, traceable, verifiable, easy to modify.

### Design
During the design phase, the requirements specification is transformed into design suitable for coding (implementation) in some programming language. 
There are two commonly used design approaches: 
#### Traditional Approach: 
Consists of two activities, **Structured Analysis** and **Structured Design**. 
**Structured Analysis** is carried out using Data Flow Diagrams (DFDs). After structured analysis, carry out **Structured Design**.
1. High-Level Design (architectural design): decompose the system into modules, represent inter-relationships among the modules
2. Low-Level Design (detailed design): different moduels designed in greater detail, data structures and algorithms for each module are designed. 

#### Object Oriented Approach
First identify various objects (real world entities) occuring in the problem and identify the relationships among objects.
OOA has various advantages such as:
- Lower development effort
- Lower development time
- Better maintainability

### Coding and Unit Testing
During this phase, each module is implemented into some programming language as per the design done at an earlier phase. Each module is unit tested (tested independently) and then debugged & documented appropriately.

#### Integration and System Testing
At this phase, different modules are integrated in a planned manner. Modules are usually integrated through a number of steps. During each integration step, the partially integrated system is tested. 
After all the modules have been successfully integrated and tested, system testing is carried out. 

The goal of system testing is to ensure that the developed system functions according ot the its requirements as specified in the [[Software Requirements Specification (SRS)]] document. 

### Maintenance
Requires much more effort than product development. Development to maintenance effort is usually $40:60$ . 
There are mainly 3 types of maintenance:
1. **Corrective Maintenance**: Correct errors which were not discovered during the product development phases
2. **Perfective Maintenance:** Improve implementation of the system and enhance functionalities of the system
3. **Adaptive Maintenance:** Port software to a new environment, such as a new computer or operating system
