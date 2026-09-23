## Database
- Collection of related data.
- Divided into two types `structured(RDBMS)` & `unstructured(web-pages)`.

## DBMS
- Tool used to perform operations on the database.
- MySQL, PostgreSQL, Oracle, MongoDB.

<br>

## **File System vs DBMS**

| File System | DBMS |
|---|---|
| Stores data in files and folders. | Stores data in a structured database. |
| Data is usually managed by the operating system. | Data is managed by the DBMS. |
| Has more chances of duplicate data. | Helps reduce data duplication. |
| Searching data is slow. | Searching data is generally faster. |
| Need exact location of data which we want to search. | No need of location we just write queries. |
| Mutiple people cannot access at the time| Mutiple people can access at the time|
| Data consistency is difficult to maintain. | Provides better data consistency. |
| Provides limited security features. | Provides better security and access control. |
| Relationships between data are difficult to manage. | Can easily manage relationships using tables and keys. |
| No proper transaction management. | Supports transactions using ACID properties. |
| Example: `.txt`, `.csv`, `.json` files. | Example: MySQL, PostgreSQL, Oracle, MongoDB. |

<br>

## **2-Tier Architecture**
- In 2-tier architecture, the client directly communicates with the database server.
  ```
  Client
    ↓
  Database Server
  ```
- Easy to understand.
- Good for small applications.
- Difficult to maintain when many clients exist.
- Security can be weaker because clients directly access the database.

<br>

## **3-Tier Architecture**

- In 3-tier architecture, the client does not directly communicate with the database.
  ```
  Client
    ↓
  Server
    ↓
  Database
  ```
- Better security because the client does not directly access the database.
- Easy to scale.
- Suitable for large applications.

<br>
<br>

# **Schema**
- A schema is the structure/logical-representation of a database.
- It defines:
  - What tables exist.
  - What columns they contain.
  - What data type each field has.
  - What relationships exist between tables.
  - Rules such as primary key, foreign key, NOT NULL, etc.

## **3-Schema Architecture**
- describes a database using 3 different levels of abstraction.
- It separates what the user sees, how the whole database is organised, and how the data is physically stored.

```
        Users
          ↓
┌─────────────────────┐
│  External Schema    │  → What each user sees
└─────────────────────┘
          ↓
┌─────────────────────┐
│  Conceptual Schema  │  → Complete logical database
└─────────────────────┘
          ↓
┌─────────────────────┐
│  Internal Schema    │  → How data is physically stored
└─────────────────────┘
          ↓
     Storage/Disk
```

### **External Schema**
- Also called `View Level`.
- Defines what a user can see.
- Different users can have different views of the same database.
- Hides unnecessary data from the user.
- Example:

  For a college database:
    ```
    Student sees:
    Name, Roll No, Branch, Marks  
    ```
  But an accountant may see:
  ```
  Name, Roll No, Fees
  ```
### **Conceptual Schema**
- Also called Logical Level.
- Describes the complete logical structure of the database(DB designer).
- Defines: Tables, Columns, Relationships, Constraints, Keys

### **Internal Schema**
- Also called Physical Level.
- Describes how data is actually stored in the storage system(DB administrators).
- Deals with things like: Files, Blocks, Indexes, Storage locations, Data structures

<br>
<br>

# **Data Independence**

- Data Independence means we can change one level of the database without affecting the next higher level.

## **Logical Data Independence**
- It means changing the logical/conceptual schema without changing the external views used by users.
- Examples of changes
  - Changing file organisation
  - Adding/removing indexes
  - Changing storage structure
  - Changing data storage location

- Example

  Initially
  ```
  Student
  ----------------
  ID
  Name
  Branch
  ```
  Later, we add:
  ```
  Email
  Phone
  ```
  An existing user view that only shows:
  ```
  Name, Branch
  ```
  can continue working without needing to change.

## **Physical Data Independence**
- It means changing the physical/internal storage without changing the logical/conceptual schema.
- Example

  Initially:
  ```
  Student Table
     ↓
  Stored in normal data files
  ```
  Later, we add an index to make searching faster:
  ```
  Student Table
     ↓
  Index + data files
  ```
  The table structure remains the same.

<br>
<br>

# **Integrity Constraints Rules**
- Integrity constraints are rules defined in a database to maintain accuracy, consistency, and reliability. 

## **Types**

## **1. Domain Constraint**
- Defines what type of value can be stored in a column.
  ```
  Age → INTEGER
  Name → VARCHAR
  Marks → 0 to 100
  ```
  So
  ```
  Age = "Hello" ❌
  Age = 20     ✅
  ```

## **2.  NOT NULL Constraint**
- A column cannot contain NULL values.
`name VARCHAR(50) NOT NULL`


## **3. Candidate Key Constraint**
- A candidate key is a minimal set of attributes (columns) that can uniquely identify each row (tuple) in a table.
- A candidate key cannot contain NULL because it must uniquely identify every row.
- A table can have multiple candidate keys.
- One candidate key is selected as the `Primary Key`.
- The remaining candidate keys are called `Alternate Keys`.
- Examples are `email`, `rollno`, `aadharno`.
 
## **4. Entity Integrity / Primary Key Constraint**
- A primary key is the candidate key selected to uniquely identify each row.
- It must be unique.
- It cannot be NULL.
- A table can have only one primary key constraint, but that primary key can contain multiple columns (`composite primary key`).

## **5. Foreign Key Constraint**
- creates a relationship between two tables.
- Attribute or set of attributes that references to the primary key of same or different table.
- Example:

  Refrenced table

  ```
  Course
  ----------------
  Course_ID  → Primary Key
  Course_Name
  ```

  Refrencing table

  ```
  Student
  ----------------
  Student_ID
  Name
  Course_ID  → Foreign Key
  ```
  If Course_ID = 10 does not exist in the Course table, a student cannot normally reference it.

## **Referential Integrity**
- ensures that a Foreign Key always refers to a valid row in another table.

### Operations on Referenced Table
- `INSERT`: Usually, no referential integrity violation. Adding a new value to the Primary Key does not affect existing Foreign Keys.

- `DELETE` :  May cause a violation if the row is being used by the referencing table.
  - `ON DELETE CASCADE`: Delete the parent row and automatically delete the related child rows.
  - `ON DELETE SET NULL`: Delete the parent row and set the related Foreign Key values to NULL.
  - `ON DELETE NO ACTION`: Do not allow the parent row to be deleted if child rows are using it.


- `UPDATE`: Updating the Primary Key of the referenced table may cause a violation if that value is being used by a Foreign Key.


### Operations on Referencing Table
- `INSERT`: May cause a violation.

- `DELETE` :  Usually, no referential integrity violation.

- `UPDATE`: May cause a violation if we update the Foreign Key to a value that does not exist in the referenced table.

## **6. Super-key**
- A `Candidate-key` + `one or more non-candidate key`.


<br>
<br>

# **ER-Model** 
- ER Model is used to design a database before creating the actual tables.
- It shows:
  - Entities → What objects/data we store
  - Attributes → Properties of those objects
  - Relationships → How entities are connected


## **1. Entity**
- An entity is a real-world object about which we want to store information.
- Examples: Student, Teacher, Course, Employee, Customer
- In an ER diagram, an entity is represented by a rectangle.
  ```
  ┌─────────────┐
  │   Student   │
  └─────────────┘
  ````

## **2. Attribute**
- An attribute describes the properties of an entity.
- For Student:
  ```
  Student
    ↓
  RollNo
  Name
  Email
  Branch
  ```
- In an ER diagram, an attribute is represented by an oval.
  ```
            (Name)
              |
  (RollNo) — Student — (Email)
              |
            (Branch)
  ```


## **3. Relationship**
- A relationship shows how two or more entities are connected.
- Example
  ```
  Student ───── Enrolls ───── Course
  ```
  Here:
  - Student → Entity
  - Course → Entity
  - Enrolls → Relationship

- In an ER diagram, a relationship is represented by a diamond.
  ```
  ┌─────────┐       ◇─────────◇       ┌─────────┐
  │ Student │───────│ Enrolls │───────│ Course  │
  └─────────┘       ◇─────────◇       └─────────┘
  ```

<br>
<br>

# **Types of attributes**
## KeyAttribute
- It uniquely identifies each & every row(Primary key).
- represented by an oval with the attribute name underlined.

## Simple Attribute 
  - Cannot be divided further. 
  - <b> Example: Age </b>

## Composite Attribute 
- can be divided into smaller attributes. 
- **Example: Name → FirstName + LastName**

## Single-valued Attribute 
- Has one value for an entity. 
- **Example: RollNo**

## Multi-valued Attribute 
- Can have multiple values
- Represented by a double oval. 
- **Example: PhoneNo**

## Derived Attribute 
- Value can be calculated from another attribute.
- Represented by a dashed oval. 
- **Example: Age can be derived from DateOfBirth.**

<br>
<br>

# **Cardinality**
- tells us how many instances of one entity can be associated with instances of another entity.

## **One-to-One(1:1)**
- one instance of Entity A can be related to only one instance of Entity B, and vice versa.
- Example: **`Employee Works in Department`**

  - ### ER Diagram

      ```
      ┌──────────┐        ◇────────┐        ┌────────────┐
      │ Employee │─── 1 ─◇  Works  ◇─ 1 ───│ Department │
      └──────────┘        ◇────────┘        └────────────┘
      ```



  - ### Base Tables

      **Employee**

      | EID | Ename | Age |
      |-----|-------|-----|
      | E1  | A     | 20  |
      | E2  | B     | 25  |
      | E3  | C     | 28  |
      | E4  | A     | 24  |
      | E5  | B     | 25  |

      **Department**

      | DID | Dname | Loc    |
      |-----|-------|--------|
      | D1  | IT    | Bang.  |
      | D2  | Prod. | Delhi  |
      | D3  | HR    | Delhi  |

  - ### Creating a Separate Relationship Table

      **Works**

      | EID | DID |
      |-----|-----|
      | E1  | D1  |
      | E3  | D2  |
      | E5  | D3  |

      Here:
      - `EID` → Foreign Key → `Employee(EID)`
      - `DID` → Foreign Key → `Department(DID)`
      - Primary Key of Works → `(EID, DID)`

      Since it's a **1:1** relationship, one employee has only one department and one department has only one employee. So in the Works table:
      - `EID` alone uniquely identifies a row
      - `DID` alone uniquely identifies a row

      That means **both** EID and DID can individually act as candidate keys in Works.



- ### Can We Remove the Works Table?

  **Yes** — because Works has no attributes of its own.

  For a 1:1 relationship, we can usually avoid a separate relationship table by putting a Foreign Key in one of the entity tables instead.

  - **Option A: Put DID inside Employee**

      | EID | Ename | Age | DID  |
      |-----|-------|-----|------|
      | E1  | A     | 20  | D1   |
      | E2  | B     | 25  | NULL |
      | E3  | C     | 28  | D2   |
      | E4  | A     | 24  | NULL |
      | E5  | B     | 25  | D3   |

    Here:
    - `EID` → Primary Key
    - `DID` → Foreign Key → `Department(DID)`, and also **UNIQUE**


  - **Option B: Put EID inside Department**

      | DID | Dname | Loc   | EID |
      |-----|-------|-------|-----|
      | D1  | IT    | Bang. | E1  |
      | D2  | Prod. | Delhi | E3  |
      | D3  | HR    | Delhi | E5  |

      Here:
      - `DID` → Primary Key
      - `EID` → Foreign Key + **UNIQUE**

  Both options are valid ways to implement the same 1:1 relationship — choose whichever side makes more sense for your queries (e.g., pick the side that will less often have NULLs).

- ###  When Should You Keep the Relationship Table?

  Keep a separate Works table if the relationship itself carries data — i.e., it has its own attributes.

  **Example:**

  ```
  Employee ─── Works ─── Department

  Works:
  - JoiningDate
  - Salary
  - Shift

  These attributes are called Descriptive attribute
  ```

  **Works**

  | EID | DID | JoiningDate | Shift |
  |-----|-----|-------------|-------|

  In this case, the relationship holds meaningful information beyond just "who connects to whom," so a dedicated Works table makes sense — even though the underlying entity relationship is still 1:1.


## **One-to-Many (1:N)**
- one instance of Entity A can be related to many instances of Entity B. But each instance of B is related to one instance of A.
- Example : **`Customer gives Order`**
  - ### ER Diagram

      ```
      ┌──────────┐          ◇─────────◇          ┌─────────┐
      │ Customer │─── 1 ───◇   Gives  ◇─── M ───│  Order  │
      └──────────┘          ◇─────────◇          └─────────┘
                                |
                              Date
      ```

      **Meaning:**
      - One Customer can give many Orders.
      - Each Order belongs to one Customer.

  - ### Base Tables 

    **Customer**

      | ID | Name | City   |
      |----|------|--------|
      | C1 | A    | Jaipur |
      | C2 | B    | Delhi  |
      | C3 | C    | Mumbai |
      | C4 | A    | Mumbai |

      `ID` → Primary Key


      **Order**

      | O_no | Item_name | Cost |
      |------|-----------|------|
      | O1   | Bucket    | 1000 |
      | O2   | Shoes     | 2000 |
      | O3   | Shirt     | 1500 |
      | O4   | Jeans     | 2000 |

      `O_no` → Primary Key

  

  - ###  Creating a Separate Relationship Table

      - The **Gives** relationship can be converted into:


        | ID | O_no | Date |
        |----|------|------|
        | C1 | O1   | ...  |
        | C1 | O2   | ...  |
        | C2 | O3   | ...  |
        | C2 | O4   | ...  |

        Here:
        - `ID` → Foreign Key → `Customer(ID)`
        - `O_no` → Foreign Key → `Order(O_no)`
        - `Date` is an attribute of the relationship itself

        **Primary Key:** Because an order belongs to only one customer, and `O_no` is already unique on its own:

        ```
        O_no → Primary Key of Gives
        ```



  - ### Can We Reduce the Number of Tables?

    - **Yes.** Because this is a **1:M** relationship, we can remove the separate Gives table by putting the Foreign Key on the **Many** side.

      ```
      Customer 1 ───── M Order
                          ↑
                        FK
      ```

      So we add `CustomerID` (and `Date`) directly into the Order table.

      **New Order Table**

      | O_no | Item_name | Cost | CustomerID | Date |
      |------|-----------|------|------------|------|
      | O1   | Bucket    | 1000 | C1         | ...  |
      | O2   | Shoes     | 2000 | C1         | ...  |
      | O3   | Shirt     | 1500 | C2         | ...  |
      | O4   | Jeans     | 2000 | C2         | ...  |

      Now:
      - `O_no` → Primary Key
      - `CustomerID` → Foreign Key
      - `Date` → description Attribute



## **Many-to-One (N:1)**
-  reverse view of 1:N.

## **Many-to-Many (M:N)**
-  many instances of Entity A can be related to many instances of Entity B.
- Example: **`Student–study-course`**

  - ### ER Diagram

    ```
    ┌─────────┐       M      ◇───────◇      N       ┌────────┐
    │ Student │──────────────◇  Study ◇──────────────│ Course │
    └─────────┘              ◇───────◇               └────────┘
    ```

      **Meaning:**
      - One Student can study many Courses.
      - One Course can be studied by many Students.


  - ### Base Tables

      **Student**

      | RollNo | Name | Age |
      |--------|------|-----|
      | 1      | A    | 16  |
      | 2      | B    | 17  |
      | 3      | A    | 16  |
      | 4      | C    | 17  |
      | 5      | D    | 15  |

      `RollNo` → Primary Key

      **Course**

      | C_id | Name  | Credit |
      |------|-------|--------|
      | C1   | Maths | 4      |
      | C2   | Phy.  | 4      |
      | C3   | Chem. | 4      |
      | C4   | Hindi | 4      |

      `C_id` → Primary Key



  - ### The Study (Relationship) Table

    - Because this is an **M:N** relationship, we need a separate relationship table — this is not optional here.

      **Study**

      | RollNo | C_id |
      |--------|------|
      | 1      | C1   |
      | 2      | C2   |
      | 1      | C2   |
      | 2      | C1   |
      | 3      | C3   |

      Here:
      - `RollNo` → Foreign Key → `Student(RollNo)`
      - `C_id` → Foreign Key → `Course(C_id)`



  - ### Primary Key of Study

    - Notice that `RollNo` alone repeats:

      ```
      1 → C1
      1 → C2
      ```

      So `RollNo` alone **cannot** be the Primary Key.

    - Similarly, `C_id` alone repeats:

      ```
      C1 → Student 1
      C1 → Student 2
      ```

      So `C_id` alone **cannot** be the Primary Key either.

    - Therefore, we combine both:

      ```
      (RollNo, C_id) → Composite Primary Key
      ```


      This combination uniquely identifies each relationship. For example:

      ```
      (1, C1) → Student 1 studies Course C1
      (1, C2) → Student 1 studies Course C2
      ```



  - ### Why Can't We Remove the Study Table?

    - The relationship is:

      ```
      Student M ───── N Course
      ```

      A student can take multiple courses, and a course can have multiple students — so a single Foreign Key column can't capture this.

    - We **cannot** do this in Student:

      ```
      Student
      ----------------
      RollNo
      Name
      C_id        ❌
      ```

      ...because Student 1 might study C1, C2, and C3 — that would need multiple values crammed into one `C_id` column, which breaks basic relational table design (no repeating groups in a single cell).

      The same problem happens in reverse if we try putting `RollNo` inside Course.
