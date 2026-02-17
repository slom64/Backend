- Its tool used for database version contorl.
- It use file in `/main/resources/db/migration`
	- It has special convesion in file name, `V1__description_is_big.sql`
	- You can dump `mysql` database using `mysqldump --user='root' --password='root' --databases 'store' --no-data --routines --triggers --result-file=V1__store_info.sql`


```mysql
CREATE TABLE profiles
(
	id 
	bio
	phone_number
	date_of_birth
	loyalty_points
	FOREIGN KEY (id) REFERENCES users(id)
);
```