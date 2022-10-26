# (STEP 12) PROJECT 5: Client / Server Architecture Using A mySQL Relational Database Management System


## IMPLEMENT A CLIENT SERVER ARCHITECTURE USING MYSQL DATABASE MANAGEMENT SYSTEM (DBMS).


To demonstrate a basic client-server using MySQL Relational Database Management System (RDBMS), follow the below instructions

Create and configure two Linux-based virtual servers (EC2 instances in AWS).

* Login to AWS EC2 Console
* Click on Launch Instance
![Launch Instance](./Images/Launch%20Instance.PNG)

* Name it Project_5
* Under Application and OS Images select Ubuntu
* Select Ubuntu 22.04 LTS Free Tier
* You can use an existing key but in this case we will create a new key.

  *  Click create new key
  * Name the key whatever you want in out case PBL Project 5
  * Leave everything as defaulted and click create key pair.
* Under Security, leave Create a New Security Group as defaulted.


  ![Create New Security Group](./Images/Create%20New%20Security%20Group.PNG)

* Select 2 under Number of Instances

  ![Number of Instances](./Images/Number%20of%20Instances.PNG)

* Click Launch Instance at the bottom

* The screen below will be displayed.

  ![Instances Created](./Images/Instances%20Created.PNG)

* Click on View Instances

  ![Instances Displayed](./Images/Instances%20Displayed.PNG)

* Edit the names of the servers. One as PBL Project_5 MYSQL DB-Server and PBL Project_5 MYSQL Client.

  ![Project 5 Instances](./Images/Project%205%20Instances.PNG)


## *TASK* – Implement a Client Server Architecture using MySQL Database Management System (DBMS).



On mysql server Linux Server install MySQL Server software.


Interesting fact: MySQL is an open-source relational database management system. Its name is a combination of "My", the name of co-founder Michael Widenius’s daughter, and "SQL", the abbreviation for Structured Query Language.


Run `sudo apt update -y`


![sudo apt update_DB_Server](./Images/sudo%20apt%20update_DB_Server.PNG)


Run `sudo apt install mysql-server -y`


![Install mysql_server_db_server](./Images/Install%20mysql_server_db_server.PNG)


We need to enable the service by running the command below.


`sudo systemctl enable mysql`


![Enable mysql_db_server](./Images/Enable%20mysql_db_server.PNG)



On mysql client Linux Server install MySQL Client software.



Start the Client and connect.

Run `sudo apt update -y`


![sudo apt update_client](./Images/sudo%20apt%20update_client.PNG)


Run `sudo apt install mysql-client -y`


![sudo apt install mysql_client](./Images/sudo%20apt%20install%20mysql_client.PNG)



*By default, both of your EC2 virtual servers are located in the same local virtual network, so they can communicate to each other using local IP addresses. Use mysql server's local IP address to connect from mysql client. MySQL server uses TCP port 3306 by default, so you will have to open it by creating a new entry in ‘Inbound rules’ in ‘mysql server’ Security Groups. For extra security, do not allow all IP addresses to reach your ‘mysql server’ – allow access only to the specific local IP address of your ‘mysql client’.*


* Click on Instance and select on PBL Project_5 DB-Server > Security > Security groups. Click on Secury groups.


![Edit Security_DB_Server](./Images/Edit%20Security_DB_Server.PNG)



Click on edit inbound rules.


![Edit Inbound Rules](./Images/Edit%20Inbound%20Rules.PNG)


Click on Add Rule.


![Add Rule](./Images/Add%20Rule.PNG)


Select MYSQL/Aurora under Type. Under Source you need to put the IP address of the Client. You can get the IP address from EC2 or run the command ` ip addr show`



![IP Address Client](./Images/IP%20Address%20Client.PNG)


The IP address for the Client is currently 172.31.32.229/20.


![Edit Inbound Rules2](./Images/Edit%20Inbound%20Rules2.PNG)



Click on Save Rules. The below screen will be displayed.


![Save Rules](./Images/Save%20Rules.PNG)


We need to create a database on mysql server (PBL Project_5 DB-Server).


Run `sudo mysql_secure_installation` in PBL Project_5 DB-Server . This  helps to prepare mysql server instance.


![Mysql Secure Installation](./Images/Mysql%20Secure%20Installation.PNG)


For this set-up we will use *GxT927&mB5* as password. We will select Yes.



![Validate Password](./Images/Validate%20Password.PNG)


Select whichever level you want. In my case I will select 1 for medium.


Select Y if you wish to continue with the password. I will click Y to continue.


![Continue with Password](./Images/Continue%20with%20Password.PNG)


After putting the password several times , I received the error below.


![Set Password Error](./Images/Set%20Password%20Error.PNG)


Follow the setps below to resolve the error.

* Run `sudo mysql` 

![sudo mysql_db_server](./Images/sudo%20mysql_db_server.PNG)


* Run `ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password by 'GxT927&mB5';`

* *Note* : You need to give it a new password that you want based on the policy that you selected before. I selected 1 which is medium.


* Exit mysql .

  ![Exit mysql_db_server](./Images/Exit%20mysql_db_server.PNG)


* Now run `sudo mysql_secure_installation` again and it should finish without errors. You will be prompted to input password. Input the password you created , in our case *GxT927&mB5*

* You will be asked to change password for root. Select any letter if you do not want to change . I selected k and and pressed enter.

* You will be prompted to remove enonymous users. I selected y and enter for yes.


  ![Mysql Secure Installation2](./Images/Mysql%20Secure%20Installation2.PNG)


* Disallow Root login remotely, select Y for yes.


  ![Disallow Root Login Remotely](./Images/Disallow%20Root%20Login%20Remotely.PNG)


* Remove test Databases and access to it - Y for Yes


  ![Remove Test Databases and Access](./Images/Remove%20Test%20Databases%20and%20Access.PNG)


* Reload Privilege Tables Now - Y for yes.

  ![Reload Privilege Tables Now](./Images/Reload%20Privillege%20Tables%20Now.PNG)


* All done screen will be displayed.'

  ![All Done Screen](./Images/All%20Done%20Screen.PNG)


Run `sudo mysql`. You might run into an error below like I did.

*ERROR 1045 (28000): Access denied for user 'root'@localhost' (using password:NO)*

To resolve the issue, run `mysql -u root -p`

![Mysql Login](./Images/Mysql%20login.PNG)


Create remote_user by running the command;

`CREATE USER 'remote_user'@'%' IDENTIFIED WITH mysql_native_password BY 'GxT927&mB5';`



![Create Remote User_DB_Server](./Images/Create%20Remote%20User_DB_Server.PNG)


Now we need to create the database.

Run `CREATE DATABASE test_db;`


![Create Database_DB_Server](./Images/Create%20Database_DB_Server.PNG)


Next we need to grant all priviledges on the database named test_db.

`GRANT ALL ON test_db.* TO 'remote_user'@'%' WITH GRANT OPTION;`


![Grant Priviledges](./Images/Grant%20Priviledges.PNG)


Flush the priviledges 

`FLUSH PRIVILEDGES;`


![Flush Privileges](./Images/Flush%20Privileges.PNG)


Exit 


![Exit](./Images/Exit.PNG)


At this time the user and database are created.


You might need to configure MySQL server to allow connections from remote hosts.


Run;

`sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf`


![Mysql Database Server Configuration File](./Images/Mysql%20Database%20Server%20Configuration%20File.PNG)


Replace ‘127.0.0.1’ to ‘0.0.0.0’ like this:


![Mysql Database Server Configuration File2](./Images/Mysql%20Database%20Server%20Configuration%20File2.PNG)


Save and quit by following the steps below'
* Esc
* Shift :
* wq
* It will show :wq at the bottom. 
* Enter




![Save Configuration File](./Images/Save%20Configuration%20File.PNG)


We now need to restart the service by running the command below.

`sudo systemctl restart mysql`


![Restart mysql](./Images/Restart%20mysql.PNG)



From mysql client Linux Server (PBL Project_5 Client) connect remotely to mysql server Database Engine without using SSH. You must use the mysql utility to perform this action.


* First connect PBL project_5 Client through EC2.

  ![Connect PBL Project_5 Client](./Images/Connect%20PBL%20Project_5%20EC2.PNG)


  ![Connect PBL Project_5 EC2_2](./Images/Connect%20PBL%20Project_5%20EC2_2.PNG)


  Now run `sudo mysql -u remote_user -h 172.31.34.229 -p`

  Note that the 172.31.34.229 is the Private ip address fro the PBL project_5 DB_Server. -p will allow you to be prompted for password which you had created fro root. The password is *GxT927&mB5*


  The screen below will show that we are connected from mysql client Linux Server remotely to mysql server Database Engine without using SSH.

   ![Mysql Server Database Connection](./Images/Mysql%20Server%20Database%20Connection.PNG)


   We now need to confirm that we can perform SQL queries. Run the command below.

   `Show databases;`

   ![Show databases](./Images/Show%20databases.PNG)


   Project completed.















