## Database Design and Build - Part 1


### PROBLEM STATEMENT:
There have been several questions from students, faculty, and staff on the topic of courses and curriculum in the UVA SDS Online MSDS Program.

### SOLUTION:
Construct a database containing some information about the courses. In particular, these things must be in the database:
1) Learning Outcomes (LOs) for each course
   * This data is provided. LOs capture what students should be able to do after taking a course (e.g., design and build a MySQL database).
   * LOs vary in terms of detail (they were written by different instructors).

2) The instructors assigned to teach the different courses for each term in 2021 (Spring, Summer, Fall)

### PROCESS:

First, to identify what tables we need to build and the relationships between them, we started with an ER flow diagram using classical Chen notation. This was best to represent and understand the ternary relationship between Course (information about the course subject), Instructor, and the Term.
* An instructor can teach one or more courses (subjects) in one or more terms
* A term can have multiple instances of the same course and the same course can be taught in multiple terms

Hence, the ternary relationship. We also have a relationship between Course and learning Outcomes where there are many outcomes for a course but only one course per outcome. Therefore, we have a many to one relationship between Outcomes and Course. 

Again, this was done to conceptually understand the relationships. Now, we have to convert this ternary relationship into a Relational Model. The following blog helped us in executing that: https://vertabelo.com/blog/ternary-relationship/. 
   
![ER_flow](https://github.com/eltsvetk/rrm3nh_DS5111su24_lab_02/blob/main/ER_flowdiagram.png)

We used mermaid js to create our Relational Model. 

In order to create a Relational Model that has a ternary relationship, we replace the diamond shape relationship with its own entity, then we relate it to the existing three entities (Course, Instructor, Term). We call the new entity an assigned "Class" with its assigned Course (subject), Instructor, and Term. 

1) An instructor can teach multiple classes but each class has only one instructor
   * One to many relationship between Instructor and Class
2) A course/subject can be taught in multiple classes but each class focuses on one course/subject
   * One to many relationship between Course and Class
3) A term can include multiple classes and multiple terms can have the same class (same instructor and same course)
   * Many to many relationship between Term and Class

Therefore, we derive the following tables and assigned a primary key or foreign key as needed:

### TABLES AND KEYS:
 1) Instructor table derived from the Instructor entity
    * with the attribute instructor_id as the primary key
 2) Course table derived from the Course entity
    * with the attribute course_id as the primary key
 3) Term table derived from the Term entity
    * with the attribute term_id as the primary key
 4) Class table drived from the Class entity
    * 3 attributes are BOTH the primary key and foreign key
    * From Instructor-Class: instructor_id
    * From Course-Class: course_id
    * From Term-Class: term_id
  5) Outcomes table derived from the Outcomes entity
     * with attribute outcome_id as the primary key
       
### NOTE: 
If a course will be taught again, it will be flagged as active. If the course won’t be taught again, it will be flagged as inactive. There is an attribute is_active to fulfill this requirement. Similarly, we have an is_active flag for the learning outcomes table as well since learning outcomes can change. Also, we have an attribute is_employed that tracks if an instructor is a current employee or not, similar to the is_active flag.

```mermaid
erDiagram
COURSE ||--|{ CLASS: assigned
 COURSE {
        int course_id PK
        string name
        string description
        boolean is_active
    }
OUTCOME }|--|| COURSE: has
 OUTCOME {
        int outcome_id PK
        int course_id
        string description
        boolean is_active
    }

 INSTRUCTOR ||--|{ CLASS: assigned
 INSTRUCTOR {
        int instructor_id PK
        string name
        boolean is_employed
    }

TERM }|--|{ CLASS: assigned
TERM {
        int term_id PK
        string term_name
    }

 CLASS {
        int term_id PK, FK
        int course_id PK, FK
        int instructor_id PK, FK
    }

```

### NORMALIZATION

Is there anything to normalize in the database?

Our database design passed First Normal Form (1NF). Below are the qualifications for 1NF, as described in the Beginning Database Design Solution textbook:
1) Each column must have a unique name - passed
2) The order of the rows and columns doesn't matter - passed
3) Each column must have a single data type - passed
4) No two rows can contain identical values - passed: every table has a primary key
5) Each column must contain a single value - passed
6) Columns cannot contain repeating groups - passed

Our database design also passed Second Normal Form (2NF):

1) It is in 1NF - passed
2) All of the non-key fields depend on all of the key fields - passed

Our database design lastly passes the Third Normal Form (3NF):

1) It is in 2NF - passed
2) It contains no transitive dependencies (when one non-key field's value depends on another non-key field's value) - passed

We have ensured that there are no dependencies (or columns) that are not directly related to the primary key of the table. Therefore, each table passes for Third Normal Form.

We will stop at 3NF since it provides "the biggest bang for our buck" according to the textbook. Our database design passed 1NF, 2NF, and 3NF. 

### INDEXES




---

Confirmation that above process description includes:
* ER flow diagram and Relational model is included
* tables we plan to build
* For each table, we've included the primary key and/or the foreign key and which ones we will us
* Learning outcomes, courses, and instructors have a flag for if currently active or not
* Database design passed 1NF, 2NF, and 3NF

6) (1 PT) Are there indexes that you should build? Explain your reasoning.

7) (2 PTS) Are there constraints to enforce? Explain your answer and strategy.
For example, these actions should not be allowed:
- Entering learning objectives for a course not offered by the School of Data Science
- Assigning an invalid instructor to a course


9) (2 PTS) Suppose you were asked if your database could also support the UVA SDS Residential MSDS Program. Explain any issues that might arise, changes to the database structure (schema), and new data that might be needed. Note you won’t actually need to support this use case for the project.
