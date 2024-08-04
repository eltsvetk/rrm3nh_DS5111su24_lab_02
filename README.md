## Database Design and Build - Part 1

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

TERM ||--|{ CLASS: assigned
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
