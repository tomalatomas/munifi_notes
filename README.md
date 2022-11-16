# PA103 Object-oriented Methods for Design of Information Systems

**Use at your own risk**

---

# Lecture #1: Introduction to Object-Oriented Design

## Challenges of SW development

- **Maintainability**
  - Ability to maintain/adapt the code due to bug fixes, new feature requirements
- **Quality**
  - Effeciency, security, reliability or another qualitative attributes that we can guarantee with our software
- **Complexity**
  - Current systems are too big to maintain them as whole
  - Solution to this is that we decompose the system into smaller parts and maintain each part separately
- **Changing requirements**
  - User Requirements are changing all the time
  - Possible solutions
    - System decomposition - Better localizing changes thanks to isolated parts with clear responsibilities
    - Process management - Management for every part of change (requirements management, development managent, test management) will improve maintainabilty
    - Quality assurance - We are focusing on quality of changes with regular quality testing to improve overall software quality

### System decomposition

We use modeling to control complexity of system by organizing and decomposing system into smaller parts that then have relationships between each other.

We use divide and manage (divide at impera) principle when modeling.
![](images/lecture1/system.png)

#### Structured modeling

Models that help us to decompose and organize system can be focused separately on **data** or **functions**

- Context diagram, data flow diagram, events, functional requirements, ...
- Entity-relationship diagram,data vocabulary, ...

Functional hierarchies and data clustering help to organize functional and
data models.

Continuous elaboration of models (e.g. constistency checking) can be **within** (this model is correct)  or **between** (these model corresponds to that model) models

Relationships between functional and data models can still be too complex

![](images/lecture1/strucdecomp.png)

#### Object Oriented modeling

Models are focused on objects that encapsulate data and fuctions. Complexity is then hidden inside the object. There are higher requirements on interpart communication protocals.

In OO modeling we have much more diagrams than in structured modeling. These models can be used only in special cases and sometimes they are focused on single phase in software development cycle.

Two key models that are core of object oriented development are **Class Diagram** and **Use Case Diagram**.

Consistency checking are again between and inside the model.

![](images/lecture1/oodecomp.png)

### Quality assurance

Quality consists of many qualitative attributes from different points of view

- User Experience (View of Customer) - Usability, Reliability, Performace, Security
- Code Quality (View of Developer) - Modularity, Complexity, Testability
- Long Term View (View of manager) - Adaptability, Portability, Reusability

These quality attributes can conflict themselves and there is no way to optimaze all at once.

- ![](images/lecture1/qaconflicts.png)
  - Purple relationship - Improving one decreases second
  - Blue relationship - Improving one improves second

Quality can be improved by

- Standardization of processes
- Regular testing and code reviews
- Using software patterns
  - Different paterns can be used to improve different (Functional or nonfuctional) quality attributes

### Process management

Defining and managing development processes - How to do things, how to communicate... (Tactics, methodologies, guidelines)

## OO Fundamentals

Expectations from OO Paradigm are Decomposition into small maintainable parts (Works only if we follow encapsulation rules) and reusability (does not work tell for object and classes)

**Object**

- smallest unit combining (encapsulating) data and functions
- stores data in field behind the “layer” of functions (operations)
- instantiating classes
- Methods define behaviour. Data in object defines object state. Object state can affect behaviour

Classes represent static view (design-time entities), while objects represent dynamic view (run-time entities)

Functions define **responsibility** - Object or class have defined data and methods to manipulate with

### OO decomposition principles

#### Abstraction

Abstraction in modeling is a problem of choosing what object and classes represent in the system. That affects distribution of responsibilities.

Abstraction of a single class is influenced by

- Attributes (possible states)
- Methods (behavior)
- Surrounding context – “neighboring” classes

#### Inheritance

Objects are childs of other objects. Child then inherits behaviour of inherited class.

Objects can never change affiliated class (i.e. the type) during their life time -> Never use inheritance if object’s role can vary in time

Inheritance can be always replaced by association

#### Association

Object consists of parts. Different parts distinguish object between themselves.

Association is more flexible than inheritance because links are created (and can be changed)
at run-time

Real OO decomposition usually mixes both inheritance and association together

#### SOLID Principle

- Single-responsibility principle
  - Every class, function or module should have responsibilty over single part of systems functionality
- Open-closed principle
  - Class, function or module should be open for extension but closed for modification
  - Changes will be hangled as new methods or classes and will involve minimal changes to existing code
- Liskov substitution principle
  - Introduces polymorphism
  - Whenever an instance of a class is expected, one can always substitute an instance of any of its sub-classes
    - If S is a subtype of T, then objects of type T in a program may be replaced with objects of type S without altering any of the desirable properties of that program
  - Liskov principle can be violated by replacing object with a type that has different desired behaviour (e.g. Duck that quacks is replaced with Mechanical duck that quacks only when it has batteries)
  - To preserve substitution, we have to follow several constraints put on
    - method signatures,
    - behavioral conditions of sub-types.
  - **Method signatures**
    - **Arguments** of derived classes must be equally or more generic - less derived than arguments of base class
      - ![](images/lecture1/methodsignatures.png)
    - **Return type** should be equally or less generic - more derived - than those required by the base class
      - ![](images/lecture1/methodsignatures2.png)
    - No new **exceptions** should be thrown by methods of the sub-type, only existing one or their subtypes
  - **Behavioral conditions of sub-types**
    - **History constraint**: New or modified method or properties should not modify the state of an object in a manner that would not be permitted by the base class.
      - Violation
        - ![](images/lecture1/historyconstraint.png)
        - Mechanical Duck for example
    - **Preconditions** cannot be strengthened in a sub-type
    - **Postconditions** cannot be weakened in a sub-type
    - **Invariants** of the supertype must be preserved in a sub-type
  
- Interface segregation principle
  - Large interfaces are split into smaller ones so client can work only with methods that are of interest to them
- Dependency inversion principle
  - Instead of having direct dependencies it may be better to loosely couple software modules
    - High-level modules should not import anything from low-level modules. Both should depend on abstractions
    - Abstractions should not depend on details. Details (concrete implementations) should depend on abstractions.

## Classes vs. data entities (ERD vs. class diagram)

 (Only showcase of possible implementation solutions to given diagram)

![](images/lecture1/erdclass.png)

- Identity in ERD = primary key; Identity in OO = address in the memory!
- This model is directly implementable. As opposed to ER model, M:N relationships pose no problem.

![](images/lecture1/erdclass2.png)

 Approach 1, model 1: We prefer one direction

- Company stores persons (employees) in array
- Person has no link to its companies
- Problem
  - There are many companies registered in the system. Where
they are stored?
  - How we get link to concrete address if we have no query mechanism
- Solution
  - Implement JobsMngr that stores all the companies and mediates access to companies and their employees ![](images/lecture1/erdclass3.png)

Approach 2, model 1: Bidirectional association

![](images/lecture1/erdclass4.png)

- Pros: Clear responsibilities. Responsibilities are uniformly distributed to all
classes
- Cons: Very complicated memory management and consistency checking,
especially without automatic “garbage collection”

![](images/lecture1/erdclass5.png)

Approach 2, model 2: Preserved bidirectional association, responsibility
located in a big “God” object.

- Pros: Management of instances and then the consistency checking are
localized in the JobsMngr => maintainability.
- Pros: Efficiency
- Problem: Where to put salaries
- Solution
  - ![](images/lecture1/erdclass6.png)
  - Helper class

As shown, there are many solutions. Designer has to choose best solution for given context. Patterns can help designers to make right decisions.

# Lecture #2: Analysis Patterns

## Motivation and history

### Generic Properties of Patterns

Common goal - improve the quality of SW design, either specific (security, reliability, etc.) or maintainability.

When using patterns, designers have to make decisions, usage of pattern is not mechanical -> It is creative process

- Appropriate pattern has to be selectad to reach given goals
- Sometimes we have to combine multiple patterns
- Consequences of pattern usage has to be considered

### History

Christopher Alexander (an architect) defined the pattern term for designing homes, buildings, and towns.

- Pattern: solution of the problem in given context
- “Each pattern describes a problem which occurs over and over again in our environment and
then describes the core of the solution to that problem, in such a way that you can use this
solution a million times over, without ever doing it in the same way twice.”

In 1995 book by Gamma, Helm, Johnson, Vlissideas was issued "Design patterns: Elements of Reusable object oriented software". Book focuses on patterns for generic object oriented programming,

In 1997 Martin Fowler: Analysis patterns book was published.
Fowler’s patterns are

- Intended to be used in the requirements analysis phase of the SW life cycle
- Based on the „data modeling“ (ERD or class diagram)
- Related to the decomposition of the problem domain model

SW Development Life Cycle

1) Requirement Analysis
   - Key UML models are **Use Case Diagram** and **Conceptual class diagram** (problem domain model)
2) Design
3) Development
4) Testing
5) Maintenance

### Conceptual Class Diagram

![](images/lecture/ccd.png)

- Provides basic terms and data that have to be stored in the system
  - Describes the problem domain by terms (**classes**), their basic properties (**attributes**) and contexts (**associations with cardinality**)
- Although the class diagram is a static model, in the analysis phase it’s used primarily for  the clarification of behavior (functional requirements)
- Clarifies terminology

Problem Domain Model Example

- As said, we focus on classes, associations with cardinality and this model is then used for further step by step decomposition
![](images/lecture2/pdmexample.png)

## Analysis Patterns

### Analysis patterns of M. Fowler

Defined 9 basic pattern collections covering various business domains

- **Accountability**
  - organizational structure and responsibilities
  - defines and asociates data with actors
- **Observations and Measurements**
  - measured values
- **Observations for Corporate Finance**
  - analysis of complex financial relationships and results in companies
  - Modules of managers
- **Referring to Objects**
  - objects identification
- **Planning**
  - schedules and protocols for repetitive plans
- Another pattern collections:
  - **Inventory and Accounting** (cashflow and invetorying)
  - Trading
  - Derivative Contracts
  - Trading Packages

Every collection consists of evolutionary sequence of patterns, from very basic to complex solutions

65 analysis patterns and 21 support patterns in total

### Accountability

The concept of accountability applies when a **person or organization is responsible to another person/organization**.

It captures **responsibilities** of **actors** (user roles) that often appear in the class diagram (problem domain model)

List of patterns:

- **Party**
- **Organization Hierarchies**
- **Multiple Organization Hierarchies**
- **Organization Structure**
- **Accountability**
- **Accountability Knowledge Level**
- Party Type Generalization
  - Adding generalization to party types makes it easier to define the knowledge level
- Hierarchic Accountability
  - Improves handling of constrains in organization structures.
- Operating Scopes
  - Define the responsibilities that are taken on when an accountability is created.
- Post
  - Another party type. Posts are used when accountability and scopes are defined by the post and do not change when the holder of the post changes.

#### Party

It is part of other more advanced patterns.

This model creates super type (nadtyp) for two independent analytical classes (Person, Organization) that have some shared context. Supertype then handles this context they share as one.

Use this pattern when:

- There is common behavior between two roles
- There is no need to distinguish between two roles

![](images/lecture2/party.png)

#### Organization Hierarchies

Companies have usually hierarchical organization structure (Operation Unit has Regions, Region has Divisions, Division has Sales Offices)
![](images/lecture2/orghier0.png)

Pattern uses recursive relationships
![](images/lecture2/orghier.png)
This allows very easy changes on organization structure. Without pattern, changes in model are required. With pattern, only new subclasses and rules are required
![](images/lecture2/orghier1.png)

The danger with the recursive relationship is that it allows a division to be part of a sales office - we need to specify which type is above and below which => sub-classes and constraints are introduced (as shown in the image)

#### Multiple Organization Hierarchies

![Alt text](images/lecture2/unistructure.png)

Many types we have parallel organization structures (Academic structure vs Administrative structure of University, Production structure vs Service structure)

![Alt text](images/lecture2/MOHpattern.png)

Using pattern of Organization Hierarchies for multiple structures gets hard to use with many associations. Organization Structure pattern is introduced

#### Organization Structure

![](images/lecture2/orgstruct.png)

Pattern of Org. Hier. is extended with class insted of self reflection (recursion). Two relationships then describe parent and subsidiary organization structures (Described on university structure image as lines). After introducing extending analytical class, we can associate another data with it (e.g. OrganizationStructuryType for classifying the line, TimePeriod).

Summary for university organization hierarchies image:

- OrganizationStructure represents lines
- OrganizationStructureType represents color (type) of individual lines
- Organization represents rectangles

Example:

- Two organizational structures of the company: sale vs. PR
- From the sale point of view, there are two shops, in Brno and Prague. Brno sales office is subordinated to Moravia division, Prague sale office is subordinated to Bohemia division.
- From the PR point of view, both sale offices are subordinated to the common central office
![Alt text](images/lecture2/orgstrexample.png)

Suitable for **multiple** organizational hierarchies or hierarchies **changing in time** (and we want to capture these changes)

- Overkill for single organizational hierarchy
- Adding a new hierarchy = adding a new instance of OrganizationStructureType
  
TimePeriod allows us to record changes in the organization structure over
time.

Adding a new hierarchy is adding a new instance of
OrganizationStructureType

Constraints to organization structure are expressed by referring to
properties of the organization structure

- Disadvantage: Adding a new structure type can require changing the rules
- Solution: Adding a Rule class

![Alt text](images/lecture2/OrgStructRule.png)

#### Accountability

Accountability helps to define responsibilities between two types of object (object in Party). This can be limited for given time period. The responsibilities are baseod on Commissioner-Responsible relationship (e.g. Employer-employee)

![Alt text](images/lecture2/acc.png)

- Idea is to generalize Organizational structure into more generic cases
  - Organization Structure shows relationships between organization units
  - In general people have often similar relationship to other people or organization structures
- Accountability pattern combines a generalizes the principles of Organization Structure and Party

-Each instance of Accountability represents a link between two parties, the AccountabilityType indicates the nature of the link. 

This allows you to handle any number of organizational relationships

"Accountability then allows division of work and responsibilities"?

![Alt text](images/lecture2/accexample.png)

In the example we have 3 parties, New England, Sales and BostonSales. Boston sales is in relationship with New England and Sales with relationship types as regional

Difference to Organization Hierarchy is that instead of saying “who are the parents of Boston
Sales” you can say “who are the regional parents of Boston Sales”.

#### Accountability Knowledge Level

If there are many more accountability types than there would be organization structure types => introducing knowledge level

Allows us to better distribute constraints between parties.

![Alt text](images/lecture2/accknow.png)

Connection rules says what party types we can connect (if we can connect particular party types).

Each accountability type has a set of connection rules.

Knowledge level: Which accountability types, what party types and connection rules are possible in the system.

Accountability is then relationship between two party types with accountability type.

![Alt text](images/lecture2/accknowex.png)

### Observations and Measurements

Storage and maintenance of quantitative and qualitative data

List of patterns:

- **Quantity** – values and their units
- **Conversion Ratio** – conversion ratios between units
- **Compound Units** – compound units e.g. km/h
- **Measurement** – measurement and recording of obtained values
- **Observation** – general observations and their recording
- **Protocol** – protocols of regular measurements
- **Subtyping Observation Concepts**
- Another patterns:
  - Dual Time Record ()
    - Different occurring and recording times/periods of events
  - Reject Observation
    - Observations cannot be deleted if a full audit trail is needed.
  - Active Observation, Hypothesis, and Projection
    - As observations are recorded, many levels of assurance are given.
  - Associated Observation
    - Ways to record the chain of evidence behind a diagnosis
  - Process Observation
    - Behavior models for observations

#### Quantity

Quantity pattern allows to store values with units attached to them.

By combining numbers and their units modeled as objects, we can describe how to convert quantities with conversion ratio
![Alt text](images/lecture2/quant.png)

#### Conversion Ratio
If we use more units with one value, we might want to convert between these units. This pattern defines exchange table with multiplication ratio for converting
![Alt text](images/lecture2/conrat.png)

- Problem: How to convert non-linear units, e.g. Celsius to Fahrenheit, days to months, etc.?
- Solution: Employ the Individual Instance Method pattern, e.g. replace ConversionRatio with Strategy GoF pattern.

![Alt text](images/lecture2/extable.png)

- Problem: How to convert compound units, e.g. km/h to m/s?
- Solution: Compound Units pattern.

#### Compound Unit

![Alt text](images/lecture2/compunit.png)

Unit Reference has to have more then one Unit reference or one with exponential >1 or <0

#### Measurement

![Alt text](images/lecture2/measur.png)

**PhenomenonType**: things to be measured, e.g. height, weight, blood glucose level, etc.
in a hospital.

**Measurement**: concrete measured value

**Person**: example of data assigned to the measurement

This pattern enables to record only quantitive values. To record non-quantitative values (like blood group - A+, A0, A-) we need to introduce **Observation pattern** 

#### Observation

![Alt text](images/lecture2/observ.png)

**PhenomenonType**: the same as before, e.g. blood group

**Phenomenon**: possible values, e.g. A+, A-, 0, …

**CategoryObservation**: Concrete observed qualitative value. It is similar to the
measurement but has a general phenomenon instead of quantity.

Pattern is useful for generalizing measurement and clarifying what will be measured.

**Task**: How to model a “low oil level in a car” observed in a car service?
- PhenomenonType = „oil level“
- Phenomenon = „over-full“, „OK“, „low
- Observation = Instance that links the car to the low phenomenon.

Example: We want to record many data about drivers so that we can check their reliability e.g.
- Driving period
- Driven vehicle (car, bus or tram)
- Number of accidents per year 

Solution:
PhenonemonTypes - Driven vehicle, driving period, numberofaccident
Phenomenon - car bus or tram
Measurement - hours, kilometers
Quantity - value of record

#### Protocol

the method by which the observation were made.
- person's body temperature can be measured in the mouth, armpit, or rectum

![](images/lecture3/proto.png)

#### Subtyping Observation Concepts

![](images/lecture3/subtyp.png)

If an observation is made of the presence of the subtype, then all supertypes are also considered to be present.

If an observation is made of the absence of a subtype, then that implies neither presence nor absence of the supertype.

- Diabetes has two subtypes: type I and type II. An observation that type I diabetes is
present for J. Smith implies that diabetes is also present for J. Smith. Absence of type I diabetes does not mean that J. Smith is not diabetic.

### Observations for Corporate Finance

How to analyze, gather something from business domain (data).

The goal of these patterns are not to record values of objects, but to record combinantions and groups of values by given criterias.

Extension of the Observations and Measurements collection of
pattern focused on (financial) balance of companies.

Patterns:

- **Enterprise Segment** 
- Measurement Protocol – how measurements can be calculated from other
measurements using formulas that are instances of model types
- Range – range between two quantities + operations with ranges
- Phenomenon with Range

#### Enterprise Segment

Allows to divide enterprise to parts we want to measure - these parts are created dynamically based on defined point of views - **dimensions**.
Point of view can be foe example type of manufactured goods, geographical location, product range...

![](images/lecture3/entseg.png)

Dimension = hierarchy of dimension elements, i.e. the criteria for dividing enterprise
DimensionElement: Node in the dimension hierarchy, e.g. West Coast area 
GeographicDimensionElement, …: root nodes of dimension hierarchies.

Task: We need analytical module in which we can investigate
- significant delays on lines
- Delays by vehicle type (buses, trams, trolleybus)
- Delays by geographic location
- Same for accidents

Solution:
![](images/lecture3/entsegex.png)

### Referring to Objects

### Planning

### Inventory and Accounting
 
We want to track movement of some units (e.g. money) between places (e.g. accounts).
Thanks to this collection of patterns we can declare which accounts to track for changes and causes for this changes.

The entries record each change to the account.

Modeling Principle: To record a history of changes to a value use an account for that value

Patterns:

- **Account**
- **Transaction**
- **Summary Account**
- Memo Account
- Posting Rules
- … and many more

#### Account 

This patterns serves as a container with common value change. Thanks to this pattern we have information about current value (account balance) and history of changes.
![](images/lecture3/accnt.png)

#### Transaction

Account pattern allows tracking of changes only, not causes for the change. This pattern extension of Account allows connection of accounts and tracking movement of values (money). This is used for items that cannot be created or deleted on their own, only moved. 

Task: We need to know how many vehicles are located in each depot after the day shift

We can use this to track changes of vehicles located at this depot by tracking leaving and incoming vehicles. 

![](images/lecture3/transac.png)

### Referring to Objects

Conceptual thinking about object identity, i.e., references to objects that
humans use.

This collection of patterns addresses naming objects, comparing them based on name, grouping and dividing them.

Patterns:

- **Name** – objects identification by names
- **Identification Scheme** – brings context to objects identification
- **Object Merge** –
- **Object Equivalence**–

#### Name

![](images/lecture3/nam.png)

There are 3 ways to name a object. 
- First possibility that object has one name. 
- Second is that one object can have multiple names. 
- Third is that object has one unique identification number.

