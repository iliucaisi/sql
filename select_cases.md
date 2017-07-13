1. Find all students who have taken a required course.
```
select * from taken, required where taken.course = required.course
```
```
select * from taken where course in (select course from required)
```
2. Find all students who can graduate (i.e., who have taken all required
courses) 
```
select student from ( select student, count(student) as c from taken, course where taken.course = taken.course group by student) T where T.c >= 2;
```
