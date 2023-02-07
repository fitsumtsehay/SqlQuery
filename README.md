# SQL Query Questions

#### 1. Create Table person and address.

#### CREATE TABLE person (
  	person_id INT NOT NULL,
  	first_name  VARCHAR(100) DEFAULT NULL,
  	preferred_first_name  VARCHAR(100) DEFAULT NULL,
  	last_name VARCHAR(100) NOT NULL,
  	date_of_birth DATE DEFAULT NULL,
  	hire_date DATE  DEFAULT NULL,
  	occupation VARCHAR(1) DEFAULT NULL,
  	PRIMARY KEY (`person_id`));

#### CREATE TABLE address(
	address_id INT(10) NOT NULL PRIMARY KEY,
    	person_id  INT(10) NOT NULL REFERENCES person (person_id),
    	address_type VARCHAR(4) NOT NULL,
	street_line_1 VARCHAR(100),
    	city VARCHAR(100),
    	state VARCHAR(100),
   	 zip_code VARCHAR(30));

#### INSERT INTO person (person_id, first_name, preferred_first_name, last_name, date_of_birth, hire_date, occupation)
     VALUES ('1', 'John', 'Johnny', 'Smith', '1995-05-01', '2005-04-02', '1'),
    ('2', 'Susan', 'Suzie', 'London', '1989-08-04', '2015-05-06', '4'),
    ('3', 'Michelle', 'Mischa', 'Williams', '1985-03-05', '2020-07-02', '2'),
    -('4', 'Charles', 'Charlie', 'Whitaker', '1895-07-01', '2009-02-01', '3');
    
#### 2. Write a query to select all rows from person.  If the person row has a value in preferred_first_name, select the preferred name instead of the value in first name.  Alias the column as REPORTING_NAME.

 ####Query:
SELECT person_id, first_name, preferred_first_name, last_name, date_of_birth, hire_date, occupation,
CASE WHEN person.preferred_first_name is NULL THEN first_name
ELSE person.preferred_first_name
End AS Reporting_Name
From person;

#### Output:
![image](https://user-images.githubusercontent.com/63597726/217131100-187f398a-5673-41ef-a467-f56f1c357cb9.png)

#### 3. Write a query to select all rows from person that have a NULL occupation.

#### Sample Table:
person_id	first_name	preferred_first_name	last_name	date_of_birth	hire_date	occupation
1	John	Johnny	Smith	5/1/1995	4/2/2005	S	
2	Susan	NULL	London	8/4/1989	5/6/2015	I	
3	Michelle	Mischa	Williams	3/5/1985	7/2/2020	D	
4	Charles	Charlie	Whitaker	7/1/1969	2/1/2009	F	
5	Boby	Bob	Boyce	9/4/1990	1/1/2023	NULL	
6	Franchesca	Franny	Gage	3/4/2000	2/3/2022	NULL	
							

#### Query:
SELECT person_id, first_name, preferred_first_name, last_name, date_of_birth, hire_date, occupation
FROM person
WHERE occupation is NULL;

#### Output:
5	Boby	Bob	Boyce	1990-09-04	2023-01-01	
6	Franchesca	Franny	Gage	2000-03-04	2022-02-03	
						
#### 4. Write a query to select all rows from person that have a date_of_birth before August 7th, 1990.  

#### Sample Table:        
person_id	first_name	preferred_first_name	last_name	date_of_birth	hire_date	occupation
1	John	Johnny	Smith	5/1/1995	4/2/2005	S
2	Susan	NULL	London	8/4/1989	5/6/2015	I
3	Michelle	Mischa	Williams	3/5/1985	7/2/2020	D
4	Charles	Charlie	Whitaker	7/1/1969	2/1/2009	F
5	Boby	Bob	Boyce	2/4/1990	1/1/2023	NULL
6	Franchesca	Franny	Gage	3/4/1980	2/3/2022	NULL

#### Query:
SELECT person_id, first_name, preferred_first_name, last_name, date_of_birth, hire_date, occupation
FROM person
WHERE (date_of_birth < '1990-08-07');

#### Output:
person_id	first_name	preferred_first_name	last_name	date_of_birth	hire_date	occupation
2	Susan	NULL	London	8/4/1989	5/6/2015	I
3	Michelle	Mischa	Williams	3/5/1985	7/2/2020	D
4	Charles	Charlie	Whitaker	7/1/1969	2/1/2009	F
5	Boby	Bob	Boyce	2/4/1990	1/1/2023	NULL
6	Franchesca	Franny	Gage	3/4/1980	2/3/2022	NULL


#### 5. Write a query to select all rows from person that have a hire_date in the past 100 days.

#### Sample Table:
person_id	first_name	preferred_first_name	last_name	date_of_birth	hire_date	occupation
1	John	Johnny	Smith	5/1/1995	4/2/2005	S
2	Susan	NULL	London	8/4/1989	5/6/2015	I
3	Michelle	Mischa	Williams	3/5/1985	7/2/2020	D
4	Charles	Charlie	Whitaker	7/1/1969	2/1/2009	F
5	Boby	Bob	Boyce	2/4/1990	1/1/2023	NULL
6	Franchesca	Franny	Gage	3/4/1980	2/3/2022	NULL

#### Query:
select *, DATEDIFF(current_date(), hire_date)as 
DiffDay from person
where DATEDIFF(current_date(), hire_date) between
1 and 100 order by hire_date desc

#### Output:
person_id	first_name	preferred_first_name	last_name	date_of_birth	hire_date	occupation	DiffDay
5	Boby	Bob	Boyce	2/4/1990	1/1/2023	NULL	28





#### 6. Write a query to select rows from person that also have a row in address with address_type = 'HOME'.

#### Query: 
SELECT * 
FROM person
JOIN address
ON person.person_id = address.person_id AND address.address_type = 'Home'

#### Output: 
 
#### 7. Write a query to select all rows from person and only those rows from address that have a matching billing address (address_type = 'BILL').  If a matching billing address does not exist, display 'NONE' in the address_type column.

#### Query: 
SELECT * 
FROM person
LEFT JOIN address
ON person.person_id = address.person_id AND address.address_type = 'Bill'

#### Output: 
![image](https://user-images.githubusercontent.com/63597726/217129832-88029e09-af73-4864-bbd8-748f3c891d0f.png)


#### 8. Write a query to count the number of addresses per address type.
#### Query: 
SELECT address_type, COUNT(*)
FROM yale.address
GROUP BY address_type;
Output:
address_type	COUNT(*)
Home	3
Bill	2

#### 9. Write a query to select data in the following format:

	Address_type           Home and Billing Address

#### Query: 
SELECT address_type, street_line_1 As Home_and_bill_Address
FROM address
WHERE address_type = 'home' 
Union
SELECT address_type, street_line_1
FROM address
WHERE address_type = 'bill'

#### Output:
Address_type	Home and Billing Address
Home	8660 Rolling Rock Ln	
Home	100 King Street	
Home	11921 Tildenwood Drive
Bill	7701 Woodmont Ave
Bill	4414 Willard Ave	

#### 10. Write a query to update the person.occupation column to ‘X’ for all rows that have a ‘BILL’ address in the address table.

#### Query: 
UPDATE person
INNER JOIN address
ON person.person_id = address.person_id
SET person.occupation = 'X'
WHERE address.address_type = 'Bill'

#### Output:
 ![image](https://user-images.githubusercontent.com/63597726/217129438-c7aef2a6-786f-4e2a-810f-f054c09d9735.png)
