# SQL Assessment 1

## hypothetical Student Information System (SIS)

### database comprising tables for Students, Courses, Enrollments, Teachers, and Payments.

#

#### creating database

```sql
create database SIS
use SIS
```

#### creating table Students

```sql
create table Students(
    student_id INT PRIMARY KEY,
	first_name  VARCHAR(max),
	last_name  VARCHAR(max),
	date_of_birth DATE,
	email VARCHAR(max),
	phone_number BIGINT
	);


INSERT INTO Students values
    (101,'divya','gangarapu','2003-03-04','divya.gangarapu@gmail.com',9347127599),
	(102,'shashidhar','reddy','2002-09-30','shashi309@gmail.com',8989379292),
	(103,'raghav','sanjay','2002-01-21','sanju212@gmail.com',9897439232),
	(104,'sai','chandana','2002-02-03','saichandana12@gmail.com',8179479889),
	(105,'sumanth','bachu','2003-09-15','sumanth.bachu9@gmail.com',8963829292),
	(106,'akshitha','mandana','2003-09-30','akshitha@gmail.com',9876937929),
	(107,'hema','sri','2003-07-30','hema@gmail.com',9119379292),
	(108,'ashritha','mopuri','2002-10-21','ashritha482@gmail.com',8989328742);

SELECT * FROM STUDENTS;
```

![alt text](<Screenshot 2024-06-15 144045.png>)

#### creating table Courses

```sql
create table Courses(
    course_id INT PRIMARY KEY,
	course_name  VARCHAR(max),
	credits INT,
	teacher_id INT FOREIGN KEY REFERENCES Teachers(teacher_id)
	);


INSERT INTO Courses values
    (1,'Python',5,1001),
	(2,'Java',4,1002),
	(3,'Javascript',5,1003),
	(4,'SQL',5,1004),
	(5,'Aws',5,1005),
	(6,'Git',2,1006);
SELECT * FROM Courses;
```

![alt text](<Screenshot 2024-06-15 204607.png>)

#### creating table Enrollments

```sql
create table Enrollments(
    enrollment_id INT PRIMARY KEY,
	student_id  INT FOREIGN KEY REFERENCES  Students(student_id) ,,
	course_id INT FOREIGN KEY REFERENCES Courses(course_id),
	enrollment_date DATE
	);
INSERT INTO Enrollments values
    (201,101,1,'2024-01-01'),
	(202,102,3,'2024-01-01'),
	(203,103,4,'2024-02-01'),
	(204,105,1,'2024-01-02'),
	(205,106,5,'2024-03-01'),
	(206,107,6,'2024-01-21'),
	(207,104,2,'2024-02-11');

SELECT * FROM Enrollments;
```

![alt text](<Screenshot 2024-06-15 204933.png>)

#### creating table Teachers

```sql
create table Teachers(
    teacher_id INT PRIMARY KEY,
	first_name  VARCHAR(max),
	last_name  VARCHAR(max),
	email VARCHAR(max),
	);

INSERT INTO Teachers VALUES
    (1001,'ragav','kumar','ragavkumar@gmail.com'),
	(1002,'avinash','pille','avinash889@gmail.com'),
	(1003,'krishna','kishore','kishore342@gmail.com'),
	(1004,'padma','posa','padma232@gmail.com'),
	(1005,'anjaneyulu','g','anjaneyulu1963@gmail.com'),
	(1006,'sreenu','yadav','srenu232@gmail.com');

SELECT * FROM Teachers;
```

![alt text](<Screenshot 2024-06-15 205151.png>)

#### creating table Payments

```sql
create table Payments(
    payment_id INT PRIMARY KEY,
	student_id  INT FOREIGN KEY REFERENCES Students(student_id) ,
	amount DECIMAL(10,2),
	payment_date DATETIME
	);


INSERT INTO Payments VALUES
    (10001,101,15000.50,'2024-01-01 11:02:11'),
	(10002,101,18230.50,'2024-01-01 12:03:11'),
	(10003,102,25000.40,'2024-03-01 11:02:11'),
	(10004,102,1000.50,'2024-03-01 05:04:11'),
	(10005,103,50000.50,'2024-01-21 11:02:11'),
	(10006,104,19300.50,'2024-01-06 11:02:11'),
	(10007,105,12435.50,'2024-01-23 13:02:44'),
	(10008,106,45000.50,'2024-04-01 21:02:11'),
	(10009,107,15000.50,'2024-01-01 11:02:11');
select * from payments;
```

![alt text](<Screenshot 2024-06-15 220551.png>)

## Questions

1. Write an SQL query to insert a new student named John Doe into the "Students" table.

```sql
INSERT INTO Students VALUES
    (109,'John','Doe','2001-03-21','johndoe323@gmail.com',6887872723);
SELECT * FROM STUDENTS;
```

![alt text](<Screenshot 2024-06-15 205352.png>)

2. Write an SQL query to enroll an existing student in a course, specifying the enrollment date.

```sql
INSERT INTO Enrollments
   VALUES (208,101,3,'2024-02-01');
```

![alt text](<Screenshot 2024-06-15 212753.png>)

3. Update the email address of a teacher in the "Teachers" table.

```sql
UPDATE Teachers
    SET email='ragavkumarv@ragavkumarv.com'
	    WHERE teacher_id=1001;
```

![alt text](<Screenshot 2024-06-15 220551.png>)

4. Write an SQL query to delete a specific enrollment record, choosing based on the student and course.

```sql
DELETE FROM Enrollments
    WHERE student_id=101 AND course_id=3;
```

![alt text](<Screenshot 2024-06-15 212947.png>)

5. Update a course to assign a specific teacher using the "Courses" table.

```SQL
UPDATE Courses
    SET teacher_id=1001
	    WHERE course_id=4;
```

![alt text](<Screenshot 2024-06-15 213332.png>)

6. Write an SQL query to calculate the total payments made by a specific student.

```sql
SELECT  s.student_id,(s.first_name+' '+s.last_name) as full_name , sum(p.amount) as total_amount from Payments  p
	INNER JOIN Students s
	  ON s.student_id=p.student_id
	    GROUP BY s.student_id,s.first_name,s.last_name;
```

![alt text](<Screenshot 2024-06-15 213628.png>)

7. Retrieve a list of courses along with the count of students enrolled in each.

```sql
SELECT course_name, count(student_id) as count from Enrollments e
    INNER JOIN Courses c
	   ON c.course_id=e.course_id
	       group by course_name;
```

![alt text](<Screenshot 2024-06-15 223015.png>)

8. Find the names of students who have not enrolled in any course.

```sql
SELECT s.student_id, (s.first_name+' '+s.last_name) as student_name from Students s
  LEFT JOIN Enrollments E
    ON E.student_id=S.student_id
	   WHERE E.course_id IS NULL;
```

![alt text](<Screenshot 2024-06-15 190105.png>)

9. Retrieve the first name and last name of students, along with the names of the courses they are enrolled in.

```sql
SELECT s.first_name, s.last_name, c.course_name from Students s
  INNER JOIN Enrollments e
    ON e.student_id=s.student_id
  INNER JOIN Courses c
    ON c.course_id=e.course_id;
```

![alt text](<Screenshot 2024-06-15 214039.png>)

10. List names of teachers and the courses they are assigned to.

```sql
SELECT (t.first_name+' '+t.last_name) as teacher_name ,c.course_name from teachers t
  INNER JOIN Courses c
    ON c.teacher_id=t.teacher_id;
```

![alt text](<Screenshot 2024-06-15 214220.png>)

11. Calculate the average number of students enrolled in each course using aggregate functions and subqueries.

```sql
SELECT avg(count) as avg_number from
(SELECT COUNT(course_id) as count ,Course_id from Enrollments
  group by course_id) as temporary_table
```

![alt text](<Screenshot 2024-06-15 192712.png>)

12. Identify the student(s) who made the highest payment using a subquery.

```sql
SELECT (first_name +' '+ last_name) as student_name , student_id from students
  where student_id = (
    select student_id from payments
	    where amount=(select max(amount) from payments));
```

![alt text](<Screenshot 2024-06-15 194140.png>)

13. Retrieve a list of courses with the highest number of enrollments using subqueries.

```sql
select course_id,course_name from courses
    where course_id in(
select course_id from Enrollments
	group by course_id  having count(student_id) = (
	   select max(e_count) from (
	     select count(student_id ) as e_count from Enrollments
			    group by course_id )as temp) );
```

![alt text](<Screenshot 2024-06-16 234509.png>)

14. Calculate the total payments made to courses taught by each teacher using subqueries.

```sql
select teacher_id ,(first_name+' '+last_name) as teacher_name,
    (select sum(amount) from Payments p
	     where p.student_id in(
		    select e.student_id from Enrollments e
			   where e.course_id in ( select c.course_id from courses c
			        where c.teacher_id=t.teacher_id))) as total_payment from teachers t;
```

![alt text](<Screenshot 2024-06-15 220834.png>)

15. Identify students who are enrolled in more than one course.

```sql
select student_id from enrollments
   group by student_id
      having count(course_id )>1;
```

![alt text](<Screenshot 2024-06-15 221909.png>)
