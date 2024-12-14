
#### **File Systems for Data Storage: Drawbacks & Limitations**

File systems, while simple, present significant challenges when used to manage complex data:

- **Data Redundancy & Inconsistency:** Multiple file formats and data duplication lead to storage waste and potential errors when updates are not synchronized.
- **Access Difficulty:** Retrieving specific information requires custom programs, hindering flexibility and increasing development time.
- **Data Isolation:** Data scattered across files with varying formats makes integration and analysis difficult.
- **Integrity Problems:** Constraints (e.g., ensuring valid account balances) are buried in code, making updates and enforcement challenging.
- **Atomicity Issues:** Partial updates due to failures can leave data in an inconsistent state.
- **Concurrency Challenges:** Simultaneous access by multiple users can lead to conflicts and inconsistencies.
- **Security Risks:** Controlling access to specific data elements is difficult, potentially exposing sensitive information.

**Database Management Systems (DBMS) as a Solution:**

DBMS address these issues by providing a structured, centralized approach to data management, ensuring data integrity, consistency, and security, while simplifying access and enabling concurrent operations.


#### **Levels of Abstraction in DBMS:**

- **Physical Level:**
    - Lowest level
    - Deals with physical data storage on disk (file organization, indexing)
    - Not exposed to users
- **Logical Level:**
    - Defines database structure (tables, attributes, relationships)
    - Used by database designers and administrators
    - Example: Defining a "instructor" record with ID, name, department, and salary fields
- **View Level:**
    - Highest level
    - Presents simplified view to end-users
    - Hides unnecessary details
    - Can be customized for different users or security purposes

![[Pasted image 20240709113635.png]]


#### **Instances and Schemas in DBMS**

- **Analogy:** Like types (schema) and variables (instances) in programming.
- **Schema:**
    - Logical structure of the database (blueprint).
    - Defines tables, attributes, relationships, and constraints.
    - Example: Describing the structure of "customers" and "accounts" tables and their link.
    - Types: Physical schema (physical design) and logical schema (logical design).

```
logical schema

Book (Title, Author, ISBN) 
Member (Name, MemberID, Address) 
Borrows (MemberID, ISBN, BorrowDate, DueDate)
```

```
Physical schema

Book table: Heap file, clustered index on ISBN 
Member table: Sorted file on MemberID 
Borrows table: Heap file, non-clustered index on (MemberID, ISBN)
```

- **Instance:**
    - Actual data stored in the database at a specific time.
    - Changes over time as data is added, modified, or deleted.
- **Physical Data Independence:**
    - Ability to change the physical storage (physical schema) without impacting applications that rely on the logical schema.


#### **Data Models**

Data models provide a framework for organizing and understanding information in databases:

- **Relational Model:** Organizes data into tables with rows and columns. Most widely used model.
- **Entity-Relationship (ER) Model:** Used primarily for database design, focusing on entities (things) and their relationships.
- **Object-based Models:**
    - Object-Oriented: Represents data as objects with attributes and methods.
    - Object-Relational: Combines aspects of both object-oriented and relational models.
- **Semi structured Data Model (XML):** Handles flexible, self-describing data with tags.
- **Older Models (less common):**
    - Network Model
    - Hierarchical Model


#### **DML vs. DDL**

| Feature           | Data Manipulation Language (DML)        | Data Definition Language (DDL) |
| ----------------- | --------------------------------------- | ------------------------------ |
| **Purpose**       | Manipulates existing data               | Defines database structure     |
| **Also Known As** | Query Language                          | Schema Language                |
| **Classes**       | Procedural, Declarative (Nonprocedural) | N/A                            |
| **Examples**      | SELECT, INSERT, UPDATE, DELETE          | CREATE, ALTER, DROP, TRUNCATE  |
| **Impact**        | Changes data within tables              | Changes table structure        |
| **Reversibility** | Typically reversible (transactions)     | Often irreversible             |
| **Who Uses**      | End-users, application developers       | Database administrators        |
**Key Points:**

- DML is the language used to interact with data within the existing structure of a database.
		**Procedural:** User specifies steps on how to retrieve data.
		**Declarative (Nonprocedural):** User only specifies what data is needed, not how to get it.
- DDL is used to define and modify the structure of the database itself (the "blueprint").
- SQL is the most widely used language for both DML and DDL, combining both types of commands.(non-procedural language)


#### Object-Relational Data Models:

- **Hybrid Approach:** Combines the best of relational and object-oriented paradigms.
- **Key Features:**
    - Complex Attributes: Tables can have attributes with non-atomic values (nested structures, arrays, etc.).
    - Object Orientation: Supports concepts like inheritance, encapsulation, and methods for richer modeling.
    - Declarative Access: Maintains SQL-like query languages for ease of use.
    - Upward Compatibility: Works with existing relational databases and tools.

#### XML (Extensible Markup Language):

- **Flexible Data Format:** Uses custom tags to describe the structure and meaning of data.
- **Human-Readable:** Easy for both humans and machines to understand and process.
- **Self-Describing:** Tags provide context and meaning to data elements.
- **Widely Used:** Foundation for modern data exchange formats (e.g., web services, configuration files).
- **Tool Support:** Numerous tools available for parsing, browsing, and querying XML data

#### Storage Management in DBMS:

- **Role:** Interface between low-level data storage and application programs/queries.
- **Responsibilities:**
    - Interacts with the file manager (operating system component).
    - Handles efficient storage, retrieval, and updating of data.
- **Key Concerns:**
    - **Storage Access:** Methods for accessing data on disk (e.g., sequential, random).
    - **File Organization:** How data is structured within files (e.g., heap files, sorted files).
    - **Indexing and Hashing:** Techniques for speeding up data an Efficient query processing involves finding the optimal execution plan by considering alternative strategies and their associated costs. access through indexes and hash tables.

#### **Query Processing**

![[Pasted image 20240709125019.png]]

Efficient query processing involves finding the optimal execution plan by considering alternative strategies and their associated costs.


● A **transaction** is a collection of operations that performs a single logical function in a database application
● **Transaction-management component** ensures that the database remains in a consistent (correct) state despite system failures (e.g., power failures and operating system crashes) and transaction failures.
● **Concurrency-control manager** controls the interaction among the concurrent transactions, to ensure the consistency of the database

![[Pasted image 20240709134353.png]]


#### **Database Architecture**

Database architecture is the overall design and structure of a database system, and it's heavily influenced by the computer system on which it runs. Here are the main types:

- **Centralized:** All data and processing reside on a single, central machine.
- **Client-Server:** Data is stored on a server, and clients (applications) access it remotely.
- **Parallel (Multi-Processor):** Data and processing are distributed across multiple processors within a single machine.
- **Distributed:** Data is stored and processed across multiple interconnected machines.


![[Pasted image 20240709134644.png]]


[[Chapter 2 - Intro to Relational Model]]