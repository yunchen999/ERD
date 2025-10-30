# bigER VS Code Extension - AI Agent Context Guide

## Overview
bigER is a VS Code extension for creating Entity-Relationship (ER) diagrams using a domain-specific language. This guide provides comprehensive documentation for assisting users with ER model creation, syntax, and notation support.

---

## File Structure & Basic Syntax

### File Extension
- **Extension**: `.erd`
- **Language**: erdiagram

### Basic File Structure
```
// Header (required)
erdiagram ModelName

// Notation selection (optional)
notation=default

// Model elements
entity EntityName { ... }
relationship RelationshipName { ... }
```

### Comments
```
// Single-line comment

/*
   Multi-line comment
   Can span multiple lines
*/
```

---

## Notations

bigER supports four different ER diagram notations. Each notation visualizes the same model structure differently.

### Available Notations

| Notation Type | Description | Syntax |
|---------------|-------------|---------|
| `default` | Default bigER notation | `notation=default` |
| `chen` | Chen's notation (academic style) | `notation=chen` |
| `bachman` | Bachman notation (data flow emphasis) | `notation=bachman` |
| `crowsfoot` | Crow's Foot notation (industry standard) | `notation=crowsfoot` |

### Notation Characteristics

#### Default Notation
- bigER's native representation
- Balanced between simplicity and detail

#### Chen Notation
- **Created by**: Peter Chen (1976)
- **Visual Style**: 
  - Rectangles for entities
  - Diamonds for relationships
  - Ovals for attributes
- **Cardinality**: Numbers (1, N, M) beside entity lines
- **Best for**: Academic contexts, conceptual modeling, stakeholder discussions
- **Characteristics**: Comprehensive, verbose, excellent for educational purposes

#### Bachman Notation
- **Created by**: Charles Bachman
- **Visual Style**:
  - Arrows show directionality
  - Circles and lines indicate cardinality
  - Named relationships
- **Best for**: Data flow diagrams, network/hierarchical database models
- **Characteristics**: Emphasizes directionality, top-down structure, legacy systems

#### Crow's Foot Notation
- **Also known as**: Information Engineering (IE) notation
- **Created by**: Clive Finkelstein (late 1970s)
- **Visual Style**:
  - Entities as rectangles
  - Relationships as labeled lines
  - Three-pronged "crow's foot" symbol for "many" side
  - Circle for optional (zero)
  - Bar for mandatory (one)
- **Best for**: Commercial database design, rapid development
- **Characteristics**: Most popular industry standard, intuitive, easily understood by non-technical users
- **Advantages**: Shows both minimum and maximum cardinality visually, most readily accepted in modeling sessions

---

## Entities

### Strong Entity (Basic)
```
entity EntityName {}
```

### Entity with Attributes
```
entity Employee {
    employee_id key
    name
    salary
    age derived
}
```

### Weak Entity
A weak entity depends on a strong entity for identification.

```
entity Department {
    dept_id key
    dept_name
}

weak entity Project {
    project_code partial-key
    project_name
}

weak relationship belongs_to {
    Project -> Department
}
```

**Key Differences**:
- Strong entities use `key` attributes
- Weak entities use `partial-key` attributes
- Weak entities require a weak relationship to a strong entity

### Entity Inheritance (Experimental)
```
entity Vehicle {
    manufacturer
    cost
}

entity Car extends Vehicle {
    serial_nr
}
```

---

## Attributes

### Syntax
```
attributeName
attributeName: datatype
attributeName classifier
attributeName: datatype classifier
```

### Examples
```
// Basic attribute
name

// With datatype
email: VARCHAR(255)
age: integer
price: DECIMAL(10,2)

// With classifier
employee_id key
partial_id partial-key
calculated_age derived

// Combined
employee_id: INTEGER key
birth_date: DATE
calculated_age: INTEGER derived
### Attribute Classifiers

| Classifier | Description | Usage |
|------------|-------------|-------|
| (none) | **Simple attribute** | Default, no special classification |
| `key` | **Key attribute** | Uniquely identifies entities in strong entities |
| `partial-key` | **Partial key** | Used for weak entities; identifies within context of owner entity |
| `derived` | **Derived attribute** | Can be computed from other attributes (e.g., age from birth_date) |

### Datatype Examples
Common datatypes that can be used:
- `INTEGER`, `INT`
- `VARCHAR(n)`, `CHAR(n)`
- `TEXT`, `STRING`
- `DECIMAL(p,s)`, `NUMERIC(p,s)`
- `DATE`, `TIME`, `DATETIME`, `TIMESTAMP`
- `BOOLEAN`, `BOOL`
- Custom types as needed

---

## Relationships

### Basic Relationship Types

#### Binary Relationship
```
relationship Works_For {
    Employee -> Department
}
```

#### Ternary Relationship
```
relationship Supplies {
    Supplier -> Part -> Project
}
```

#### Recursive (Self-referencing) Relationship
```
relationship Manages {
    Employee -> Employee
}
```

---

## Cardinality

**Cardinality** represents the **maximum** number of instances that can participate in a relationship.

### Cardinality Values
- `1` = **One** (exactly one or at most one)
- `N` = **Many** (zero or more, one or more)

### Cardinality Patterns

#### One-to-One (1:1)
```
relationship Marriage {
    Person[1] -> Person[1]
}
```

#### One-to-Many (1:N)
```
relationship Manages {
    Manager[1] -> Employee[N]
}
```

#### Many-to-Many (N:N)
```
relationship Enrolls_In {
    Student[N] -> Course[N]
}
```

---

## Participation Constraints

**Participation** represents the **minimum** number of instances required in a relationship.

### Participation Values
- `0` = **Optional** (zero or more)
- `1` = **Mandatory** (one or more)

### Participation Syntax
Use `min..max` format:
```
EntityName[min..max]
```

### Common Patterns

#### Optional-to-Mandatory (0..1 to 1..1)
```
relationship Works_At {
    Employee[0..1] -> Department[1..1]
}
```
- Employee may work at zero or one department (optional)
- Department must have exactly one relationship (mandatory)

#### Mandatory-to-Optional (1..1 to 0..N)
```
relationship Teaches {
    Professor[1..1] -> Course[0..N]
}
```
- Professor must teach exactly one course
- Course may have zero or more professors

#### Optional Many-to-Mandatory Many (0..N to 1..N)
```
relationship Enrolled_In {
    Student[0..N] -> Course[1..N]
}
```
- Student can be enrolled in zero or more courses
- Course must have one or more students

### Shorthand Equivalents
```
[1] ≡ [1..1]       // One and only one (mandatory single)
[N] ≡ [1..N]       // One or more (mandatory many)
[0..1]             // Zero or one (optional single)
[0..N]             // Zero or more (optional many)
```

---

## Roles in Relationships

Roles clarify the function of an entity in a relationship. They are especially useful for:
- Recursive relationships
- Ternary or complex relationships
- Relationships where entity roles aren't obvious

### Syntax
```
EntityName[ cardinality | "role_name" ]
```

### Examples

#### Binary with Roles
```
relationship Works_For {
    Employee[N | "worker"] -> Department[1 | "employer"]
}
```

#### Recursive Relationship with Roles
```
relationship Manages {
    Employee[N | "subordinate"] -> Employee[1 | "supervisor"]
}
```

#### Ternary with Roles
```
relationship Supplies {
    Supplier[N | "provider"] -> Part[N | "component"] -> Project[N | "consumer"]
}
```

#### Combined Participation and Roles
```
relationship Reports_To {
    Employee[0..N | "subordinate"] -> Employee[1..1 | "manager"]
}
```

---

## Weak Relationships

Weak relationships connect weak entities to their identifying (strong) entities.

### Syntax
```
weak relationship relationship_name {
    WeakEntity -> StrongEntity
}
```

### Complete Example
```
entity Building {
    building_id key
    address
}

weak entity Apartment {
    apt_number partial-key
    rent
}

weak relationship located_in {
    Apartment -> Building
}
```

**Key Points**:
- Weak entity must have a `partial-key`
- Strong entity must have a `key`
- Weak relationship shows dependency
- Weak entity cannot exist without the strong entity

---

## Complete Example Models

### University Database
```
erdiagram University

notation=crowsfoot

entity Student {
    student_id: INTEGER key
    name: VARCHAR(100)
    email: VARCHAR(100)
    enrollment_date: DATE
}

entity Course {
    course_id: VARCHAR(10) key
    course_name: VARCHAR(200)
    credits: INTEGER
}

entity Instructor {
    instructor_id: INTEGER key
    name: VARCHAR(100)
    department: VARCHAR(50)
}

relationship Enrolls {
    Student[0..N | "enrolled_student"] -> Course[1..N | "selected_course"]
}

relationship Teaches {
    Instructor[1 | "teacher"] -> Course[0..N | "taught_course"]
}
```

### Company Database with Weak Entity
```
erdiagram Company

notation=chen

entity Employee {
    ssn key
    name
    salary
    birth_date
    age derived
}

entity Department {
    dept_id key
    dept_name
    location
}

weak entity Dependent {
    dependent_name partial-key
    relationship_type
}

relationship Works_For {
    Employee[N] -> Department[1]
}

weak relationship Has_Dependent {
    Dependent -> Employee
}

relationship Manages {
    Employee[N | "worker"] -> Employee[1 | "manager"]
}
```

### Library System
```
erdiagram Library

notation=bachman

entity Member {
    member_id: VARCHAR(20) key
    name: VARCHAR(100)
    join_date: DATE
}

entity Book {
    isbn: VARCHAR(13) key
    title: VARCHAR(200)
    author: VARCHAR(100)
    publication_year: INTEGER
}

entity Publisher {
    publisher_id: INTEGER key
    publisher_name: VARCHAR(100)
    country: VARCHAR(50)
}

relationship Borrows {
    Member[0..N | "borrower"] -> Book[0..N | "borrowed_item"]
}

relationship Publishes {
    Publisher[1] -> Book[N]
}
```

---

## Best Practices

### Naming Conventions
1. **Entities**: Use singular nouns (PascalCase or snake_case)
   - ✅ `Employee`, `Customer`, `OrderItem`
   - ❌ `Employees`, `customers`

2. **Relationships**: Use verbs or verb phrases
   - ✅ `Works_For`, `Manages`, `Enrolls_In`
   - ❌ `WorksFor`, `Management`

3. **Attributes**: Use descriptive names (snake_case common)
   - ✅ `employee_id`, `first_name`, `hire_date`
   - ❌ `id`, `n`, `d`

### Model Organization
1. Define all entities first
2. Then define relationships
3. Group related entities together
4. Use comments to document complex logic

### Cardinality Guidelines
- Always specify cardinality when it's not 1:N
- Use participation constraints for clarity
- Add roles for recursive or complex relationships
- Document business rules in comments

### Notation Selection
- **Chen**: Educational, academic papers, conceptual design
- **Crow's Foot**: Commercial projects, database implementation
- **Bachman**: Data flow analysis, legacy system documentation
- **Default**: Quick prototyping, internal documentation

---

## Common Patterns & Scenarios

### Pattern 1: One-to-Many (Department-Employee)
```
entity Department {
    dept_id key
    name
}

entity Employee {
    emp_id key
    name
}

relationship Works_In {
    Employee[N] -> Department[1]
}
```

### Pattern 2: Many-to-Many (Student-Course)
```
entity Student {
    student_id key
    name
}

entity Course {
    course_id key
    title
}

relationship Enrolls {
    Student[N] -> Course[N]
}
```

### Pattern 3: Weak Entity (Building-Apartment)
```
entity Building {
    building_id key
    address
}

weak entity Apartment {
    apt_number partial-key
    rent
}

weak relationship Located_In {
    Apartment -> Building
}
```

### Pattern 4: Recursive (Employee Hierarchy)
```
entity Employee {
    emp_id key
    name
}

relationship Manages {
    Employee[N | "subordinate"] -> Employee[0..1 | "manager"]
}
```

### Pattern 5: Ternary (Supplier-Part-Project)
```
entity Supplier {
    supplier_id key
    name
}

entity Part {
    part_id key
    description
}

entity Project {
    project_id key
    name
}

relationship Supplies {
    Supplier[N] -> Part[N] -> Project[N]
}
```

---

## Troubleshooting Common Errors

### Syntax Errors

❌ **Incorrect:**
```
entity Employee
    name key
```

✅ **Correct:**
```
entity Employee {
    name key
}
```

---

❌ **Incorrect:**
```
relationship WorksFor {
    Employee[1] > Department[N]
}
```

✅ **Correct:**
```
relationship WorksFor {
    Employee[1] -> Department[N]
}
```

---

❌ **Incorrect:**
```
weak entity Dependent {
    name key  // Should be partial-key!
}
```

✅ **Correct:**
```
weak entity Dependent {
    name partial-key
}
```

---

## Quick Reference

### Entity Declaration
```
entity Name { attributes }
weak entity Name { attributes }
entity Child extends Parent { attributes }
```

### Attribute Declaration
```
attr_name
attr_name: DATATYPE
attr_name classifier
attr_name: DATATYPE classifier
```

### Relationship Declaration
```
relationship Name { Entity1 -> Entity2 }
relationship Name { Entity1 -> Entity2 -> Entity3 }
weak relationship Name { WeakEntity -> StrongEntity }
```

### Cardinality & Participation
```
Entity[1]          // Exactly one (mandatory)
Entity[N]          // One or more (mandatory many)
Entity[0..1]       // Zero or one (optional)
Entity[0..N]       // Zero or more (optional many)
Entity[1..1]       // Same as [1]
Entity[1..N]       // Same as [N]
```

### Roles
```
Entity[cardinality | "role_name"]
```

---

## Version Information

**Last Updated For**: bigER v0.3.0

**Official Documentation**:
- Language Guide: https://github.com/borkdominik/bigER/wiki/Language
- Notations Guide: https://github.com/borkdominik/bigER/wiki/Notations

---

## Summary for AI Agents

When assisting users with bigER:

1. **Always start with proper file structure** (erdiagram keyword, optional notation)
2. **Validate entity definitions** (check for proper key/partial-key usage)
3. **Verify relationship syntax** (proper arrow usage, cardinality placement)
4. **Suggest appropriate notation** based on use case
5. **Use roles for clarity** in recursive or complex relationships
6. **Follow naming conventions** for professional models
7. **Provide complete examples** when explaining concepts
8. **Explain cardinality vs participation** clearly
9. **Handle weak entities properly** (partial-key + weak relationship)
10. **Test syntax** against provided examples

Remember: bigER uses `.erd` files, requires the `erdiagram` keyword, and supports four notation styles for visualization flexibility.
