
![[Pasted image 20240709134955.png]]

#### **Attribute Types in the Relational Model**

- **Domain:**
    - The set of all possible values an attribute can take.
    - Defines the valid data type and range for an attribute.
- **Atomicity:**
    - Attribute values are typically considered indivisible (atomic).
    - This means they cannot be broken down further (e.g., a single integer, a single string).
- **Null Values:**
    - A special value (`NULL`) represents missing or unknown data.
    - Belongs to every domain, but can cause complications in operations and comparisons.

#### **Relation Schema and Instance**

- The schema acts as a template, defining what kind of data can be stored in a relation.
- The instance is the actual data stored at a given moment, and it can change over time.
- Each tuple represents a single entity or record (e.g., one instructor in the `instructor` relation).
- The combination of schema and instance provides a structured way to organize and access data in relational databases.

#### **Keys in Relational Databases:**

- **Superkey:** A set of attributes within a relation (table) that uniquely identifies each tuple (row).
    - Example: In an `instructor` table with attributes `ID`, `name`, etc., both `{ID}` and `{ID, name}` are superkeys.
    
- **Candidate Key:** A minimal superkey, meaning removing any attribute from it would no longer guarantee uniqueness.
    - Example: `{ID}` is a candidate key for `instructor`, as it's the smallest set of attributes that uniquely identifies each instructor.
    
- **Primary Key:** One candidate key chosen as the main way to identify tuples within a relation. Often used for indexing and establishing relationships.
    - Choice of primary key depends on factors like data stability, simplicity, and performance considerations.
    
- **Foreign Key:** An attribute in one relation that refers to the primary key of another relation. Establishes a link between two tables, enforcing referential integrity.
    - Consists of:
        - Referencing relation: The table containing the foreign key.
        - Referenced relation: The table whose primary key is referenced.


