##Q1.
```
select id,
type, 
status,
amount,
(amount)-avg(amount) as differnet
from account
where status = 'Active'
GROUP BY id,
type, 
status,
amount
```
```

 	id	type	status	amount	differnet
1	A100000	Basic Checking	Active	44539	0
2	A100002	Basic Checking	Active	11483	0
3	A100003	Student Checking	Active	26132	0
4	A100007	Student Checking	Active	71661	0
5	A100009	Savings	Active	65536	0
6	A100010	Savings	Active	40778	0
7	A100011	Savings	Active	57953	0
8	A100013	Basic Checking	Active	45654	0
9	A100016	Basic Checking	Active	639	0
10	A100018	Basic Checking	Active	63563	0
11	A100019	Basic Checking	Active	79771	0
12	A100020	Basic Checking	Active	34328	0
13	A100021	Savings	Active	34405	0
14	A100022	Student Checking	Active	51850	0
15	A100023	Basic Checking	Active	85337	0
16	A100025	Savings	Active	7265	0
17	A100030	Student Checking	Active	7275	0
18	A100032	Basic Checking	Active	51703	0
19	A100033	Student Checking	Active	20139	0
20	A100034	Savings	Active	66835	0

```

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
##Q5
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
 
##Q6
```
CREATE EXTERNAL TABLE employee_solution (
	  id 		int, 	  
	  fname string, 
	  lname string, 
	  address string, 
	  city string, 
	  state string, 
	  zip string, 
	  birthday string)
	LOCATION	  'hdfs://localhost:8020/user/training/problem6/employee'
 ```
```
insert into table employee_solution 
select id,
    fname,
    lname,
    address,
    city,
    state,
    zip,
    substr(birthday, 1,5)
from employee;

select * from employee_solution limit 10;

```
```
1	10000000	Deanna	Lane	900-1514 Vitae, Rd.	Lafayette	LA	97827	08/31
2	10000001	Hall	Garrett	9656 Urna Avenue	Tucson	AZ	86511	08/24
3	10000002	Lucian	Dotson	P.O. Box 277, 4808 Fusce St.	Kearney	NE	57731	08/12
4	10000003	Yuri	Sherman	Ap #399-8275 Molestie Road	Kapolei	HI	16943	08/26
5	10000004	Jaime	Griffin	Ap #647-2123 Quis Rd.	Madison	WI	51394	08/13
6	10000005	Zorita	Weber	747-9424 Orci, Av.	Hattiesburg	MS	90262	08/09
7	10000006	Mara	Meadows	517-4594 Ac, Rd.	Huntsville	AL	35374	08/11
8	10000007	Evan	Richard	P.O. Box 223, 8182 Non, Av.	College	AK	99682	08/25
9	10000008	Briar	Anderson	Ap #548-6452 Nunc Road	Cleveland	OH	90704	08/18
10	10000009	Cole	Odom	P.O. Box 962, 2496 Sodales St.	Boston	MA	27282	08/21
```
 
##Q7
```
select concat(fname," ", lname) as name 
from employee where city = 'Seattle' order by name asc;
```

``` 	name
1	Lucian Dotson
2	Zeph Horn
3	Zeph Mcclain
```
##Q8
```
sqoop export --connect jdbc:mysql://localhost/problem8 --username cloudera --password cloudera--table solution--export-dir 'hdfs://localhost:8020/user/training/problem8/customer'
```

##Q9
```
CREATE EXTERNAL TABLE solution(
 id 		int,
 fname 		string,
 lname		string,
 address 	string,
 city 		string,
 state 		string,
 zip		string,
 birthday	string
)  
row format delimited 
fields terminated by '\t'
location 'hdfs://localhost:8020/user/training/problem9/';


insert into table solution
select concat("A",id) as id
       , fname
       , lname
       ,address
       ,city
       ,state
       ,zip
       ,birthday
from customer;
```
```
 	id	fname	lname	address	city	state	zip	birthday
1	A1000000	Medge	Roach	P.O. Box 799, 6865 Nec Rd.	Racine	WI	56336	08/10/2016
2	A1000001	Nasim	Stone	P.O. Box 975, 759 Scelerisque Street	Tuscaloosa	AL	36696	08/16/2016
3	A1000002	Jolie	Schneider	P.O. Box 829, 9011 Vulputate St.	Kapolei	HI	59913	08/22/2016
4	A1000003	Lacota	Molina	Ap #831-2124 Pharetra. Avenue	Philadelphia	PA	84716	08/24/2016
5	A1000004	Blaine	Sweet	P.O. Box 151, 7847 Pede. St.	Topeka	KS	66452	08/21/2016
6	A1000005	Lesley	Bird	P.O. Box 569, 6635 Maecenas St.	Dallas	TX	87918	08/26/2016
7	A1000006	Sydnee	Howell	P.O. Box 147, 3714 Dignissim Street	Dallas	TX	94072	08/11/2016
8	A1000007	Jermaine	Griffin	Ap #859-2722 Donec Rd.	Baltimore	MD	13392	08/10/2016
9	A1000008	Brynn	Pennington	Ap #968-9936 Eleifend Avenue	Lincoln	NE	89536	08/23/2016
10	A1000009	Ava	Noble	P.O. Box 955, 1459 Urna St.	Baton Rouge	LA	34822	08/08/2016
```
