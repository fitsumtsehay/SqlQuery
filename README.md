# SQL Query Questions

### 1. Create Table person and address.

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
    
### 2. Write a query to select all rows from person.  If the person row has a value in preferred_first_name, select the preferred name instead of the value in first name.  Alias the column as REPORTING_NAME.

 #### Query:
###### SELECT person_id, first_name, preferred_first_name, last_name, date_of_birth, hire_date, occupation,
###### CASE WHEN person.preferred_first_name is NULL THEN first_name
###### ELSE person.preferred_first_name
###### End AS Reporting_Name
###### From person;

#### Output:
![image](https://user-images.githubusercontent.com/63597726/217131100-187f398a-5673-41ef-a467-f56f1c357cb9.png)

### 3. Write a query to select all rows from person that have a NULL occupation.

#### Sample Table:
![image](https://user-images.githubusercontent.com/63597726/217134930-ddbc6b92-cca9-4dad-9fd1-5283187d3e40.png)

#### Query:
###### SELECT person_id, first_name, preferred_first_name, last_name, date_of_birth, hire_date, occupation
###### FROM person
###### WHERE occupation is NULL;

#### Output:
![image](https://user-images.githubusercontent.com/63597726/217136248-b4b9e471-49e9-41d1-88d6-f9bef1d35818.png)
					
### 4. Write a query to select all rows from person that have a date_of_birth before August 7th, 1990.  

#### Sample Table: 
![image](https://user-images.githubusercontent.com/63597726/217136733-41e6e34f-1243-487f-976d-18d4e5ade748.png)

#### Query:
###### SELECT person_id, first_name, preferred_first_name, last_name, date_of_birth, hire_date, occupation
###### FROM person
###### WHERE (date_of_birth < '1990-08-07');

#### Output:
![image](https://user-images.githubusercontent.com/63597726/217137155-581fb0f0-4693-4779-b39d-b5baa9a44fec.png)

### 5. Write a query to select all rows from person that have a hire_date in the past 100 days.

#### Sample Table:
![image](https://user-images.githubusercontent.com/63597726/217138238-c80af229-530f-41f0-9c81-871e952ad673.png)

#### Query:
###### select *, DATEDIFF(current_date(), hire_date)as 
###### DiffDay from person
###### where DATEDIFF(current_date(), hire_date) between
###### 1 and 100 order by hire_date desc

#### Output:
![image](https://user-images.githubusercontent.com/63597726/217138559-ada3876d-0149-41e9-b6c2-4cb99809f58d.png)

### 6. Write a query to select rows from person that also have a row in address with address_type = 'HOME'.

#### Query: 
###### SELECT * 
###### FROM person
###### JOIN address
###### ON person.person_id = address.person_id AND address.address_type = 'Home'

#### Output: 
![image](https://user-images.githubusercontent.com/63597726/217139254-29ff4d9d-8382-485f-84b5-c873614884e3.png)
 
### 7. Write a query to select all rows from person and only those rows from address that have a matching billing address (address_type = 'BILL').  If a matching billing address does not exist, display 'NONE' in the address_type column.

#### Query: 
###### SELECT * 
###### FROM person
###### LEFT JOIN address
###### ON person.person_id = address.person_id AND address.address_type = 'Bill'

#### Output: 
![image](https://user-images.githubusercontent.com/63597726/217129832-88029e09-af73-4864-bbd8-748f3c891d0f.png)


### 8. Write a query to count the number of addresses per address type.
#### Query: 
###### SELECT address_type, COUNT(*)
###### FROM yale.address
###### GROUP BY address_type;

### Output:
![image](https://user-images.githubusercontent.com/63597726/217139705-67658584-49da-4589-a33d-dc5ccc200841.png)

### 9. Write a query to select data in the following format:
######	Address_type           Home and Billing Address

#### Query: 
###### SELECT address_type, street_line_1 As Home_and_bill_Address
###### FROM address
###### WHERE address_type = 'home' 
###### Union
###### SELECT address_type, street_line_1
###### FROM address
###### WHERE address_type = 'bill'

#### Output:
![image](https://user-images.githubusercontent.com/63597726/217140245-4bf2dced-54eb-4cc6-b0d0-954e200bbfeb.png)

### 10. Write a query to update the person.occupation column to ???X??? for all rows that have a ???BILL??? address in the address table.

#### Query: 
###### UPDATE person
###### INNER JOIN address
###### ON person.person_id = address.person_id
###### SET person.occupation = 'X'
###### WHERE address.address_type = 'Bill'

#### Output:
 ![image](https://user-images.githubusercontent.com/63597726/217129438-c7aef2a6-786f-4e2a-810f-f054c09d9735.png)
