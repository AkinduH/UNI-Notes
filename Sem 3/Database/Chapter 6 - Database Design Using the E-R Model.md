
#### **ER model - Database Modeling**

- Defines overall data structure.
- Key concepts:
    - Entity sets (real-world objects like customers, products).
    - Relationship sets (connections between entities, e.g., customer-order).
    - Attributes (properties of entities, e.g., customer name).
- ER diagrams: visual representation of the model.
- Benefits: clear communication, early problem detection, easier maintenance.

An entity is an object that exists and is distinguishable from other objects.
An entity set is a set of entities of the same type that share the same properties
A relationship is an association among several entities

![[Pasted image 20240709171243.png]]
![[Pasted image 20240709171252.png]]
![[Pasted image 20240709175407.png]]
![[Pasted image 20240709180026.png]]
![[Pasted image 20240709182618.png]]

**Representing Cardinality Constraints in ER Diagram**

We express cardinality constraints by drawing either a directed 
line (→), signifying “one,” or an undirected line (—), signifying 
“many,” between the relationship set and the entity set.

One-to-Many Relationship
A student is associated with at most one instructor via advisor and  instructor is associated with several (including 0) students.
![[Pasted image 20240709184252.png]]

Many-to-One Relationships
A student is associated with several (including 0) instructors via advisor and an instructor is associated with at most one student
![[Pasted image 20240709184535.png]]

Many-to-Many Relationship
A student is associated with several (possibly 0) instructors via advisor and a student is associated with several (possibly 0) instructors 
![[Pasted image 20240709184836.png]]

![[Pasted image 20240709184930.png]]

**Notation for Expressing More Complex Constraints**

![[Pasted image 20240709190504.png]]

![[Pasted image 20241124142033.png]]

![[Pasted image 20241124142056.png]]

![[Pasted image 20240709215448.png]]

### **Specialization**

- **Definition**: A top-down design approach where an entity set is divided into **sub-groupings** (lower-level entity sets) based on distinctive attributes or relationships.

![[Pasted image 20241124154040.png]]

#### **Schema Representation**:

1. **Separate Schemas**:
    - Higher-level entity has its own schema.
    - Lower-level entities inherit the primary key and have their own schemas.
    - Minimizes redundancy but requires joins for queries.
2. **Flattened Schemas**:
    - Each sub-group has a schema containing both inherited and unique attributes.
    - Simple queries but may lead to redundancy.


### **Generalization**

- **Definition**: A **bottom-up design approach** where multiple **lower-level entity sets** with common features are combined to form a **higher-level entity set**. This is the opposite of specialization, which is top-down.
    
- **Relation to Specialization**:
    
    - Generalization is the reverse of specialization. While specialization divides an entity set into sub-entities, generalization combines multiple sub-entities into a more generalized, higher-level entity set.

### **Completeness Constraint in Generalization**

The **completeness constraint** defines whether an entity in a higher-level entity set (resulting from generalization) **must** belong to at least one of the lower-level entity sets. This constraint specifies if the generalization is **total** or **partial**.

#### **Types of Completeness Constraints**:

1. **Total Generalization**:
    - Every entity in the higher-level entity set **must** belong to at least one of the lower-level entity sets.
    - Example: In a **Student** entity, all students must be either **Graduate** or **Undergraduate**. There is no student that is neither.
2. **Partial Generalization**:
    - An entity in the higher-level entity set **does not necessarily** belong to any of the lower-level entity sets.
    - Example: A **Person** might be an **Instructor**, **Secretary**, or neither. In this case, not all persons are required to belong to either of these categories.


