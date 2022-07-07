# MySQL-Project
Project 5

The project started with the creation of 2 Linux instance on AWS:  mysql server and mysql client.  See the snapshot of the two instances [here.](<img width="825" alt="EC2 Instances" src="https://user-images.githubusercontent.com/61512079/177770454-28e8b44f-65bd-4282-a676-65ae151fe7b5.PNG">)

Next is the installation of MySQL Server as shown below:
On the MySQL Server Instance, run the following set of commands to install the MySQL server package  -
```bash
sudo apt update
sudo apt install mysql-server
```
After the server installation, the MySQL service is started using the comand below:
```bash
sudo systemctl start mysql.service
```
Server Status shown [here.](https://user-images.githubusercontent.com/61512079/177774361-31d43b24-d4f9-4ed3-af77-48266a0b89c6.PNG)

Following the server installation is the running of DBMS security script to change the default security settings:
```bash
sudo mysql_secure_installation
```
The root account password was changed from default and confirmed with the new password.
```bash
mysql -u root -p
```
The mySQL server was connected using the root as shown [here.](https://user-images.githubusercontent.com/61512079/177778930-f3e11c79-89e1-442b-8f84-8ec5441946c4.png)

Another user was created on the MySQLserver using the command below after connecing as root:
```mysql
CREATE USER 'lekan'@'localhost' IDENTIFIED BY 'password';
```
Note the 'password' indicated was changed to a strong password to comply with the password policy.

Test the mysql instance using the new user:
```bash
sudo mysqladmin -p -u lekan version
```
**MYSQL-SERVER-TEST**\
<img width="726" alt="mysql-server-version" src="https://user-images.githubusercontent.com/61512079/177782995-ab81af84-a9cc-4dd8-87d7-17a20b18f489.PNG">

Next is the Installation of the client using the command below on the mysql client instance:
```bash
sudo apt install mysql-client -y
```
Check the client version to verify if the installation was successful:
```bash
mysql -V
```
Output:\
<img width="407" alt="mysql-client" src="https://user-images.githubusercontent.com/61512079/177784207-3e6f6c96-4fd8-4a45-883c-1d40d6f01f41.PNG">

In order to connect to the MYSQL Server instance,  port 3306 was opened on the EC2 instance security group:
<img width="831" alt="Server-Inbound-port1" src="https://user-images.githubusercontent.com/61512079/177787268-43678fe5-61d0-4840-9df1-69ad3e2b1bf1.PNG">

Next the server is edited to allow connection from remote host by changing the config file as follow:
```bash
sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf
```
<img width="604" alt="allow-remote" src="https://user-images.githubusercontent.com/61512079/177788203-32ba5386-7c49-4366-aba9-28f3bd293672.PNG">

In addition to the above, the database user must be allowed to connect to the host server by running the following commands on the MYSQL Server:
```bash
 sudo mysql  -u root -p
 ```
 ```mysql
 GRANT ALL PRIVILEGES ON *.* to 'lekan'@'172.31.27.181';
 FLUSH PRIVILEGES;
 ```
 And the host allowed for the user is confirmed by:
 ```mysql
 SELECT host FROM mysql.user WHERE user = "lekan";
 ```
 <img width="355" alt="Allow-user-host" src="https://user-images.githubusercontent.com/61512079/177813767-3f40b3d3-f373-4808-a6ab-12d46aa83fd1.PNG">
  
The connection is now tested from the client to the server using the mysql utility as follow:
```bash
mysql -u lekan -p -h 172.31.27.181
```
<img width="478" alt="access-test-from-mysql-tool" src="https://user-images.githubusercontent.com/61512079/177813966-831f7bad-0414-4719-845a-9295df54ff84.PNG">\

<img width="152" alt="DB-Show" src="https://user-images.githubusercontent.com/61512079/177814099-9a2268af-4725-4370-af36-89d196eb719f.PNG">




