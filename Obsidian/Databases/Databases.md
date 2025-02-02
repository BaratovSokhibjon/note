
## Normalization

> **The Data-integrity fail will happen if the database is designed bad and is not normalized.**

### Normalized tables:
- Easier to understand
- Easier to enhance and extend
- Protected from:
	- insertion anomalies
	- update anomalies
	- deletion anomalies

### Levels of Normalization

> These levels act as safety assessments, the higher the form is, the safer the database is.

>  Normalization helps maintain data consistency and reduces redundancy, ensuring better database performance and easier maintenance.

>Database normalization is the process of organizing data in a database to minimize redundancy and improve data integrity. Below are the five normal forms with their rules and important details:

---

#### 1. First Normal Form (1NF)
1. **Row Order**: Information must not depend on the row order; rows should be treated independently.
2. **Single Data Type per Column**: Mixing different data types (e.g., text and numbers) within the same column is not allowed.
3. **Primary Key**: Every table must have a **primary key** to uniquely identify each row.
4. **No Repeating Groups**: Data must not contain sets of related data within a single table. Each column should hold atomic values (indivisible).

**Example of a Violation**:  
A table storing multiple phone numbers in a single cell (e.g., "123456, 789012") violates 1NF.

---

#### 2. Second Normal Form (2NF)
1. **Dependency on Whole Primary Key**: Every non-key attribute must depend on the entire primary key, not just a part of it.
2. **Eliminate Partial Dependency**: For tables with a composite primary key (a primary key made of two or more columns), ensure non-key attributes depend on all parts of the key.

**Example**:  
Consider a table with a composite key (`StudentID, CourseID`). If `StudentName` depends only on `StudentID`, it violates 2NF.

![2NF Illustration](2nd-lvl-nf.png)

---

#### 3. Third Normal Form (3NF)
1. **Dependency on Primary Key**: Every non-key attribute should depend on the primary key and nothing else.
2. **No Transitive Dependency**: Non-key attributes must not depend on other non-key attributes.

**Example of a Violation**:  
A table with columns `EmployeeID`, `DepartmentID`, and `DepartmentName` where `DepartmentName` depends on `DepartmentID` instead of `EmployeeID`.

**Special Case**:  
This is also referred to as **Boyce-Codd Normal Form (BCNF)** when every determinant is a candidate key.

---

#### 4. Fourth Normal Form (4NF)
1. **No Multi-Valued Dependencies**: If a table has two or more independent multi-valued facts about an entity, they must be stored in separate tables.
2. **Dependency on the Key**: Multi-valued dependencies should only exist on the primary key.

**Example of a Violation**:  
A table storing both `ProjectAssigned` and `SkillSet` for an `EmployeeID` where these attributes are independent of each other.

---

#### 5. Fifth Normal Form (5NF)
1. **Decomposability**: A table should not contain information that can be reconstructed by joining smaller tables.
2. **Eliminate Join Dependencies**: Data must not have dependencies that require complex joins to be represented correctly.

**Requirement**:  
The table must already be in **4NF** before being evaluated for 5NF.

---

#### Summary of Normal Forms

| Normal Form | Key Rule                                                                                    |
| ----------- | ------------------------------------------------------------------------------------------- |
| 1NF         | Eliminate repeating groups and ensure atomic values.                                        |
| 2NF         | Ensure no partial dependency on a composite primary key.                                    |
| 3NF         | Eliminate transitive dependencies; every non-key attribute depends only on the primary key. |
| 4NF         | Eliminate multi-valued dependencies unrelated to the primary key.                           |
| 5NF         | Eliminate join dependencies; data should not rely on complex joins for reconstruction.      |



## Designing a database

#### First collect ==information requirements==
> Defining what kind of information does the database store 
- Example
	- Keep track of which ==people== own which ==pets==
	- Capture the ==names== of the ==owners==
	- Capture the ==names== of the ==pets==
	- Store the ==photos== of the ==pets==
	- Store the ==addresses== where the ==pets== live
	- store a field which will determine what ==type== of ==animal== each ==pet== is

> A good start is to identify the nouns in the information requirements
- people, pets, names, owners, names, pets, photos, pets, addresses, pets, type, animal, pet

> It is better to get all the nouns into singular rather than plural
- person, pet, name, owner, name, pet, photo, pet, address, pet, type, animal, pet

> Get rid of redundant & duplicate words
- person, name, pet, photo, address, pet type 
## What are ERD Diagrams?

![[chen notation.png]]
