## Database Design and Build - Part 1


PROBLEM STATEMENT:
There have been several questions from students, faculty, and staff on the topic of courses and curriculum in the UVA SDS Online MSDS Program.

SOLUTION:
Construct a database containing some information about the courses. In particular, these things must be in the database:
1) Learning Outcomes (LOs) for each course
   * This data is provided. LOs capture what students should be able to do after taking a course (e.g., design and build a MySQL database).
   * LOs vary in terms of detail (they were written by different instructors).

2) The instructors assigned to teach the different courses for each term in 2021 (Spring, Summer, Fall)

PROCESS:

First, to identify what tables we need to build and the relationships between them, we started with an ER flow diagram using classical Chen notation. This was best to represent and understand the ternary relationship between Course (information about the course subject), Instructor, and the Term.
* An instructor can teach one or more courses (subjects) in one or more terms
* A term can have multiple instances of the same course and the same course can be taught in multiple terms

Hence, the ternary relationship. We also have a relationship between Course and learning Outcomes where there are many outcomes for a course but only one course per outcome. Therefore, we have a many to one relationship between Outcomes and Course. 

Again, this was done to conceptually understand the relationships. Now, we have to convert this ternary relationship into a Relational Model. The following blog helped us in executing that: https://vertabelo.com/blog/ternary-relationship/. 
   
![ER_flow](https://github.com/eltsvetk/rrm3nh_DS5111su24_lab_02/blob/main/entity_relationship.png)

We used mermaid js to create our Relational Model. 

In order to create a Relational Model that has a ternary relationship, we replace the diamond shape relationship with its own entity, then we relate it to the existing three entities (Course, Instructor, Term). We call the new entity an assigned "Class" with its assigned Course (subject), Instructor, and Term. 

1) An instructor can teach multiple classes but each class has only one instructor
   * One to many relationship between Instructor and Class
2) A course/subject can be taught in multiple classes but each class focuses on one course/subject
   * One to many relationship between Course and Class
3) A term can include multiple classes and multiple terms can have the same class (same instructor and same course)
   * Many to many relationship between Term and Class

Therefore, we derive the following tables and assigned a primary key or foreign key as needed:

TABLES AND KEYS:
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
     *
   
  
  
 




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
        int course_id FK
        int term_id FK
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

Design Questions

1) (3 PTS) What tables should you build?

2) (2 PTS) For each table, what field(s) will you use for primary key? 

3) (2 PTS) For each table, what foreign keys will you use?

4) (2 PTS) Learning outcomes, courses, and instructors need a flag to indicate if they are currently active or not. How will your database support this feature? In particular:

If a course will be taught again, it will be flagged as active. If the course won’t be taught again, it will be flagged as inactive.

It is important to track if an instructor is a current employee or not.

Learning outcomes for a course can change. You’ll want to track if a learning outcome is currently active or not.

5) (1 PT) Is there anything to normalize in the database, and if so, how will you normalize it? Recall the desire to eliminate redundancy.

6) (1 PT) Are there indexes that you should build? Explain your reasoning.

7) (2 PTS) Are there constraints to enforce? Explain your answer and strategy.
For example, these actions should not be allowed:
- Entering learning objectives for a course not offered by the School of Data Science
- Assigning an invalid instructor to a course

8) (5 PTS) Draw and submit a Relational Model for your project. For an example, see Beginning Database Design Solutions page 115 Figure 5-28.

9) (2 PTS) Suppose you were asked if your database could also support the UVA SDS Residential MSDS Program. Explain any issues that might arise, changes to the database structure (schema), and new data that might be needed. Note you won’t actually need to support this use case for the project.
