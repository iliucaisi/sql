1. Find all students who have taken a required course.
```
select * 
from taken, required 
where taken.course = required.course
```
```
select * 
from taken 
where course in 
(
select course from required
)
```
2. Find all students who can graduate (i.e., who have taken all required
courses) 
```
select student as c 
from taken 
where taken.course in
(
	select course
	from required
)
group by student
having count(*) = 
(
	select count(*)
	from required
)
```

```
create table CannotGraduate as
select distinct student 
from 
(
	select * 
	from 
	(
		select S.student,C.course 
		from 
		(
			select distinct student 
			from taken
		) S, 
		(
			select course 
			from required
		) C
	)SC 
	where not exists 
	(
		select * 
		from taken 
		where taken.student = SC.student and taken.course=SC.course
	)
) CG;
select * from 
(
	select distinct student 
	from  taken
	where not exists
	(
		select * 
		from CannotGraduate
		where taken.student = CannotGraduate.student
	)
);
```

```
select *
from taken as x
where not exists
(
	select *
	from required as y
	where not exists
	(
		select *
		from taken as z
		where x.student = z.student and x.course = z.course
	)
)
```

