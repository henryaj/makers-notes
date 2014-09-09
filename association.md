# Association

## One-to-many

Think of blog posts and their comments. Each post has *many* comments.

## One-to-one

These are rarer. An example would be one user having one profile.

## Many-to-many

Consider hashtags on Twitter. A single hashtag can be used in many tweets, but also a single tweet can have many hashtags.

Many-to-many relationships are recorded using *join tables* which map items from one database to items in another.

In the case of students and courses, you would have two tables – one for students, one for courses – and then a join table that links student IDs with course ID.

You can let your ORM create the join table for you, or you can create it manually (which is useful if you want to add additional properties to the table, like student grade).

| ID | NAME  |
|----|-------|
| 1  | Andy  |
| 2  | Merve |
| 3  | Spike |

| ID | COURSE  |
|----|-------|
| 1  | Maths  |
| 2  | PE |
| 3  | Music |

The join table looks like this:

| STUDENT_ID | COURSE_ID  |
|----|-------|
| 1  | 1 |
| 1  | 3 |
| 2  | 1 |
| 2  | 2 |
| 3  | 1 |

