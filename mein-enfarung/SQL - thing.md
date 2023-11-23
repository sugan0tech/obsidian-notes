
### My sql kicks starter pack

## Demo db
- Having a demo production like table will be really helpful. Will be using a demo db from mysql [community](https://dev.mysql.com/doc/employee/en/). download link https://github.com/datacharmer/test_db
- table structure ![[Pasted image 20231122194700.png]]
- Then move to the directory
  ```bash
	  cd test_dp-master
  ```
  - in employees.sql update 
    ```bash
		set default_storage_engine = Maria;
    ```
- Now login to maria cli and source the file
  ```SQL
	  mariadb[none]> create database employees;
	  mariadb[none]> use employees;
	  mariadb[employees] source /home/user/test_db-master/employees.sql;
  ```
