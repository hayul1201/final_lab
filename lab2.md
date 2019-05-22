##Q2.

```
CREATE EXTERNAL TABLE employee(
 id 		int,
 fname 		string,
 lname		string,
 address 	string,
 city 		string,
 state 		string,
 zip		string,
 birthday	string,
 hireday	string
)  
row format delimited 
fields terminated by ','
location 'hdfs://localhost:8020/user/training/problem2/employee';
```

##Q3.

```
select a.custid, c.fname , c.lname, c.hphone
from account a 
    ,customer c
where a.amount < 0.0 
and a.custid = c.id;

```
```
 	a.custid	c.fname	c.lname	c.hphone
1	10001	Sybil	Wiley	(504) 780-0366
2	10010	Brittany	Martinez	(341) 462-0222
3	10011	Merritt	Roth	(341) 344-3753
4	10015	Amaya	Weeks	(979) 852-8703
5	10023	Jermaine	Barnes	(245) 674-2743
6	10026	Lacy	Rios	(227) 499-8358
7	10036	Laith	Kirk	(312) 466-1766
8	10045	Chancellor	Hart	(913) 147-2294
9	10046	Olga	Hall	(615) 645-2920
10	10053	Jonah	Alvarado	(578) 584-0882
11	10054	Brady	Cunningham	(631) 358-1802
12	10056	Yoshio	Benton	(875) 483-7221
13	10068	Maggy	Whitney	(886) 286-7557
14	10073	Ariel	Jordan	(422) 191-3529
15	10080	Margaret	Mercer	(695) 261-7712
16	10092	Christian	Baker	(659) 732-3384
17	10093	Brandon	Rhodes	(344) 635-8512
18	10094	Orli	Nolan	(459) 228-5694
19	10096	Blaze	Gill	(834) 777-5765
 
``` 
#Q5
```
select fname,lname, city, state 
from customer 
where  city = 'Palo Alto'
and state = 'CA'
union all
select fname,lname, city, state
from employee
where  city = 'Palo Alto'
and state = 'CA';
```

```
 	_u1.fname	_u1.lname	_u1.city	_u1.state
1	Farrah	Preston	Palo Alto	CA
2	Brielle	Hudson	Palo Alto	CA
3	Nelle	Kim	Palo Alto	CA
4	Tatum	Jacobs	Palo Alto	CA
5	Ivan	Gentry	Palo Alto	CA
6	Naida	Tran	Palo Alto	CA
7	Alden	Daniels	Palo Alto	CA
8	Maggy	Mcdaniel	Palo Alto	CA
9	Griffin	Pate	Palo Alto	CA
10	Burton	Hayes	Palo Alto	CA
11	Ali	Barker	Palo Alto	CA
12	Jasper	Lara	Palo Alto	CA
```
 

