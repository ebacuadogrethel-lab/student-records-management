# Student Records Management System
## Complete Documentation: DFD, ERD, and Database Schema

---

## Student Information
- **First Name:** Grethel
- **Last Name:** Alejandre
- **Email:** thelebacuado@gmail.com
- **Student ID:** CA2024560
- **Year Level:** 3rd yr
- **Course:** BSIT

---

## 1. DFD (Data Flow Diagram)

### Level 0 - System Context Diagram

```
┌─────────────────────────────────────────────────────────────┐
│                                                               │
│              Admin / Registrar User                           │
│                                                               │
└──────────────────────────┬──────────────────────────────────┘
                           │
                    Student Data
                    Requests / Updates
                           │
                           ▼
        ┌──────────────────────────────────────┐
        │                                        │
        │   Student Records Management System   │
        │                                        │
        │  - Add Student                         │
        │  - Edit Student                        │
        │  - Delete Student                      │
        │  - View Students                       │
        │  - Sort Records                        │
        │                                        │
        └──────────────────┬───────────────────┘
                           │
                    Records / Confirmations
                           │
                           ▼
        ┌──────────────────────────────────────┐
        │   Student Records Database            │
        │                                        │
        │  - Students Table                      │
        │  - Courses Table                       │
        │  - Contacts Table                      │
        │                                        │
        └──────────────────────────────────────┘
```

### Level 1 - Detailed Process Flow

```
User Input
    │
    ├─────────────────────────────────────┐
    │                                       │
    ▼                                       ▼
┌─────────────┐                    ┌──────────────┐
│  Add Record │                    │  Edit Record │
│             │                    │              │
│ Process 1.1 │                    │ Process 1.2  │
└─────────────┘                    └──────────────┘
    │                                       │
    └──────────────┬──────────────────────┘
                   │
                   ▼
        ┌──────────────────────┐
        │  Student Records DB  │
        │  (Data Store 1)      │
        └──────────────────────┘
                   │
    ┌──────────────┼──────────────┐
    │              │              │
    ▼              ▼              ▼
┌─────────────┐ ┌──────────────┐ ┌──────────────┐
│View Records │ │Sort Records  │ │Delete Record │
│             │ │              │ │              │
│ Process 2.1 │ │ Process 2.2  │ │ Process 3.1  │
└─────────────┘ └──────────────┘ └──────────────┘
    │              │                      │
    └──────────────┼──────────────────────┘
                   │
                   ▼
            Display / Output
```

---

## 2. ERD (Entity Relationship Diagram)

### Visual Layout

```
    ┌────────────────────┐
    │      COURSE        │
    ├────────────────────┤
    │ PK  course_id      │◄─────┐
    │     course_name    │      │
    │     course_code    │      │ 1 : Many
    └────────────────────┘      │
                                 │
                                 │
                           ┌─────┴────────────┐
                           │                  │
                           │                  │
                    ┌──────┴──────────┐     │
                    │    STUDENT      │     │
                    ├─────────────────┤     │
                    │ PK student_id   │────┘
                    │    first_name   │
                    │    last_name    │
                    │ FK course_id    │
                    │    year_level   │
                    └────────┬────────┘
                             │
                             │ 1 : 1
                             │
                    ┌────────▼────────┐
                    │    CONTACT      │
                    ├─────────────────┤
                    │ PK contact_id   │
                    │ FK student_id   │
                    │  phone_number   │
                    │      email      │
                    │     address     │
                    └─────────────────┘
```

### Relationship Summary

| Relationship | Type | Description |
|---|---|---|
| COURSE - STUDENT | 1 : Many | One course can have many students |
| STUDENT - CONTACT | 1 : 1 | One student has one contact record |

---

## 3. Database Schema

### Table Structure Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│                         COURSES TABLE                            │
├──────────────┬──────────────┬────────────────┬─────────────────┤
│ course_id    │ course_name  │ course_code    │ Constraints     │
│ (INT, PK)    │ (VARCHAR)    │ (VARCHAR)      │ NOT NULL, UNIQUE│
├──────────────┼──────────────┼────────────────┼─────────────────┤
│ AUTO_INCREMENT                                                  │
└─────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│                        STUDENTS TABLE                            │
├──────────────┬──────────────┬──────────────┬─────────────────────┤
│ student_id   │ first_name   │ last_name    │ course_id (FK)      │
│ (VARCHAR, PK)│ (VARCHAR)    │ (VARCHAR)    │ (INT) → courses     │
├──────────────┼──────────────┼──────────────┼─────────────────────┤
│ year_level   │ NOT NULL     │ NOT NULL     │ ON DELETE RESTRICT  │
│ (VARCHAR)    │                             │                     │
└─────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│                       CONTACTS TABLE                             │
├──────────────┬──────────────┬──────────────┬─────────────────────┤
│ contact_id   │ student_id   │ phone_number │ email               │
│ (INT, PK)    │ (VARCHAR, FK)│ (VARCHAR)    │ (VARCHAR)           │
├──────────────┼──────────────┼──────────────┼─────────────────────┤
│ address      │ UNIQUE       │ FK → students│ ON DELETE CASCADE   │
│ (TEXT)       │ NOT NULL     │              │                     │
└─────────────────────────────────────────────────────────────────┘
```

### SQL Schema

```sql
-- COURSES Table
CREATE TABLE courses (
    course_id INT PRIMARY KEY AUTO_INCREMENT,
    course_name VARCHAR(100) NOT NULL,
    course_code VARCHAR(20) NOT NULL UNIQUE
);

-- STUDENTS Table
CREATE TABLE students (
    student_id VARCHAR(20) PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    course_id INT NOT NULL,
    year_level VARCHAR(20) NOT NULL,
    CONSTRAINT fk_student_course
        FOREIGN KEY (course_id) REFERENCES courses(course_id)
);

-- CONTACTS Table
CREATE TABLE contacts (
    contact_id INT PRIMARY KEY AUTO_INCREMENT,
    student_id VARCHAR(20) NOT NULL UNIQUE,
    phone_number VARCHAR(20),
    email VARCHAR(100),
    address TEXT,
    CONSTRAINT fk_contact_student
        FOREIGN KEY (student_id) REFERENCES students(student_id)
);
```

### Field Definitions

#### COURSES Table
| Field | Type | Constraint | Description |
|---|---|---|---|
| course_id | INT | PRIMARY KEY, AUTO_INCREMENT | Unique course identifier |
| course_name | VARCHAR(100) | NOT NULL | Name of the course |
| course_code | VARCHAR(20) | NOT NULL, UNIQUE | Course code (e.g., BSIT) |

#### STUDENTS Table
| Field | Type | Constraint | Description |
|---|---|---|---|
| student_id | VARCHAR(20) | PRIMARY KEY | Unique student identifier |
| first_name | VARCHAR(50) | NOT NULL | Student's first name |
| last_name | VARCHAR(50) | NOT NULL | Student's last name |
| course_id | INT | NOT NULL, FOREIGN KEY | Reference to courses table |
| year_level | VARCHAR(20) | NOT NULL | Academic year (1st yr, 2nd yr, etc.) |

#### CONTACTS Table
| Field | Type | Constraint | Description |
|---|---|---|---|
| contact_id | INT | PRIMARY KEY, AUTO_INCREMENT | Unique contact identifier |
| student_id | VARCHAR(20) | NOT NULL, UNIQUE, FOREIGN KEY | Reference to students table |
| phone_number | VARCHAR(20) | NULLABLE | Student's phone number |
| email | VARCHAR(100) | NULLABLE | Student's email address |
| address | TEXT | NULLABLE | Student's physical address |

---

## 4. Sample Data

### Sample Records

```sql
-- Insert Courses
INSERT INTO courses (course_name, course_code) VALUES
('Bachelor of Science in Information Technology', 'BSIT'),
('Bachelor of Science in Computer Science', 'BSCS'),
('Bachelor of Secondary Education', 'BSED');

-- Insert Student Record (Grethel Alejandre)
INSERT INTO students (student_id, first_name, last_name, course_id, year_level) VALUES
('CA2024560', 'Grethel', 'Alejandre', 1, '3rd yr');

-- Insert Contact Information
INSERT INTO contacts (student_id, phone_number, email, address) VALUES
('CA2024560', '09123456789', 'thelebacuado@gmail.com', 'Cebu City');
```

### Sample Data Table

| student_id | first_name | last_name | course | year_level | phone_number | email | address |
|---|---|---|---|---|---|---|---|
| CA2024560 | Grethel | Alejandre | BSIT | 3rd yr | 09123456789 | thelebacuado@gmail.com | Cebu City |

---

## 5. Data Flow Summary

### Add Student Process
```
User Input
    │
    ▼
┌──────────────────────────────┐
│ Validate Student Data        │
│ (ID, Name, Course, Year)     │
└──────────────┬───────────────┘
               │
               ▼
        ┌──────────────────┐
        │ Insert into      │
        │ STUDENTS table   │
        └──────────────────┘
               │
               ▼
        ┌──────────────────┐
        │ Insert into      │
        │ CONTACTS table   │
        └──────────────────┘
               │
               ▼
        Display Success
        Refresh Table
```

### Sort/View Process
```
User Request
    │
    ├─ Sort by Course
    │    │
    │    ▼
    │  SELECT * FROM students
    │  JOIN courses
    │  ORDER BY course_name
    │
    ├─ Sort by Year Level
    │    │
    │    ▼
    │  SELECT * FROM students
    │  ORDER BY year_level
    │
    └─ View All
         │
         ▼
       Display All Records
```

---

## 6. System Relationships

### Functional Relationships

```
User Interface (HTML/CSS/JS)
        │
        ▼
    CRUD Operations
        │
    ├── Create (Add)
    ├── Read (View)
    ├── Update (Edit)
    └── Delete
        │
        ▼
    localStorage / Database
        │
        ├── STUDENTS table
        ├── COURSES table
        └── CONTACTS table
```

---

## 7. Constraints & Integrity

### Primary Keys
- `courses.course_id` - Uniquely identifies each course
- `students.student_id` - Uniquely identifies each student
- `contacts.contact_id` - Uniquely identifies each contact record

### Foreign Keys
- `students.course_id` → `courses.course_id` (Referential Integrity)
- `contacts.student_id` → `students.student_id` (Referential Integrity)

### Uniqueness Constraints
- `courses.course_code` - Each course must have a unique code
- `contacts.student_id` - Each student has only one contact record

---

## Conclusion

This documentation provides a complete overview of the Student Records Management System's data structure, including:
- **DFD** - Shows how data flows through the system
- **ERD** - Illustrates relationships between entities
- **Database Schema** - Defines table structures and constraints

The system efficiently manages student records while maintaining data integrity through proper relationships and constraints.
