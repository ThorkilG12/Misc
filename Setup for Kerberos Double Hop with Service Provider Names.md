## What's involved ##
This setup is a laptop PC with SSMS (SQL Server Management Service), SQL Server 1, SQL Server 2 and an Active Directory Domain Controller (DC)  
All running on Windows OS  
The domain is G12.Local and hostnames are "T1201" (with Ip:252) and "TEST2019" (with Ip:219) for SQL1 and SQL2  
(SQL1 is the "middle" and SQL2 is the one that users windows credentials should be delegated to)
## Basic Settings ##
First be sure how to reach the SQL Servers:  
![image](https://github.com/ThorkilG12/Misc/assets/12120277/b23544ca-e583-42cf-a6cd-f90f09a104eb)

In SSMS for both server, enable "Both failed and successful logins" from "Server Properties" - "Sucurity"

The user on the Laptop is `mrtest@g12.local` or G12/mrtest and he is member of the DomainUsers group

## The Data ##

On SQL2 (TEST2019) a simple table with a few rows and columns has been created: (Database "Sandbox2" lives on SQL2) 
![image](https://github.com/ThorkilG12/Misc/assets/12120277/f61d348d-d10d-4432-94a7-7cea9086820b)

In order for SQL1 to get some data from SQL2 a "Linked Server" is setup:
```
EXEC sp_addlinkedserver
    @server='BOX2', -- Name of the Linked Server
    @srvproduct='', -- Leave empty for SQL Server
    @provider='SQLNCLI', -- SQL Server Native Client
    @datasrc='TEST2019'; -- Name or IP of the remote server
```
Be sure that this is enabled for the linked server on BOX1 (T1201):  
![image](https://github.com/ThorkilG12/Misc/assets/12120277/9da9c38d-c292-4a4b-bbde-bb727f77bb51)

On SQL1 (T1201) a view is created in database "Sandbox" like this:  
```
CREATE OR ALTER VIEW [dbo].[myView1] AS
SELECT *
FROM [BOX2].[Sandbox2].[dbo].[MyTable2];
```
A table on SQL2 is part of a view that exists on SQL1.  
SQL1 "talks" to SQL2 through a "Linked Server"  
When the user opens the view on SQL1, the credentials must be passed to SQL2 in order to "see" the content of the view.

In AD a group called `G12\Test_Users` exists with one member: `mrTest`  
On SQL1 under "Security" - "Logins" this is set:  
![image](https://github.com/ThorkilG12/Misc/assets/12120277/fc55418b-132b-45d2-b0c6-a5fb75e51c67)

and for Sandbox2 the same setting exists on SQL2.

If client opens SSMS, connects to SQL1 and tries to open "MyView1" an error is generated. As expected.

## SPN and Delegation ##

In a standard SQL Express installation, the User running the SQL service is `NT Service\MSSQL$SQLEXPRESS`. Having two servers with the same owner-name is not acceptable fro SPN's to work.  
Therefore two users has been created, `SQL252` and `SQL219`, and the owner of the SQL Services for both SQL Server is changed:  
For SQL2 it looks like:  
![image](https://github.com/ThorkilG12/Misc/assets/12120277/db65d432-8cc8-4180-a3d1-b9c33760e97c)

From a CDM prompt on a server where an AD Administrator is logged in, execute command `SETSPN` four times:  
```
For SQL Server 1
setspn -S MSSQLSvc/t1201.g12.local g12.local\SQL252
setspn -S MSSQLSvc/t1201.g12.local:1433 g12.local\SQL252
```
```
For SQL Server 2
setspn -S MSSQLSvc/test2019.g12.local g12.local\SQL219
setspn -S MSSQLSvc/test2019.g12.local:1433 g12.local\SQL219
```

Now we need delegation, and we are done:


## Tricks ##
execute as login = 'G12\mrtest';
select * from fn_my_permissions(null,'database')
SELECT SUSER_NAME(), USER_NAME();
SELECT TOP (10) * FROM [dbo].[<some object>]
revert;
