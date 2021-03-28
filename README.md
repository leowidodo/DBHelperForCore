# DBHelperForCore
DB Connection Helper for .net core


Steps for using DBHelper
1.	Configure connection String in appsettings.json

   "Connection": {"DefaultConnection": "Server=(local);Database=DB_TEST;user id= leo; password= 123456"}
 
2.	Add Library DBHelperForCore in  Nuget Package
 
3.	Create new Class ConnDB.cs

  public class ConnDB
      {
          public static DBHelper connection = new DBHelper("DefaultConnection"); 
          public static DBHelper connection2 = new DBHelper("DefaultConnection2"); 
      } 
      
In this class we can define multiple connection strings.
4.	How to use it.
A.	For Insert, Delete or Update data using Stored Procedure
-	Create Stored Procedure 

CREATE PROCEDURE InsertPeople
(
@NAME VARCHAR(50),
@ADDRESS VARCHAR(100),
@PHONE_NUMBER VARCHAR(20)
)
AS
BEGIN
	INSERT INTO [dbo].[PeopleInfoTbl] (Name,Address,PhoneNumber,DateAdd)
	VALUES (@NAME, @ADDRESS, @PHONE_NUMBER, GETDATE())
END

Using in Code 
using DBHelperForCore; 

Return result


public string InsertData(String name, string Address,string PhoneNumber)
        {
            DBParams DbPrcess = new DBParams(); // Variable
            DbPrcess.AddParameter("@NAME", name); // Parameter
            DbPrcess.AddParameter("@ADDRESS", Address); // Parameter
            DbPrcess.AddParameter("@PHONE_NUMBER", PhoneNumber); // Parameter
            int DbResult = ConnDB.connection.InsertUpdateDeleteProcedure("InsertPeople", DbPrcess);// Stored procedure Name
            if (DbResult > 1)
            {
                return "OK";
            }
            else
            {
                return "NG";
            }
            
        }

Method

Public void InsertData(String name, string Address,string PhoneNumber)
        {
            DBParams DbPrcess = new DBParams(); // Variable
            DbPrcess.AddParameter("@NAME", name);// Parameter
            DbPrcess.AddParameter("@ADDRESS", Address); // Parameter
            DbPrcess.AddParameter("@PHONE_NUMBER", PhoneNumber); // Parameter
            ConnDB.connection.InsertUpdateDeleteProcedure("InsertPeople", DbPrcess); // Stored procedure Name
                        
        }
B.	For Scalar Data
-	Create Stored Procedure 
CREATE PROCEDURE TOTAL_RECORD
AS
BEGIN
	SELECT COUNT(*) AS TOTAL FROM [PeopleInfoTbl]
END

Using in Code 

using DBHelperForCore;
Return result
public int TotalRecord()
        {
            DBParams DbPrcess = new DBParams();
            int DbResult = ConnDB.connection. SelectScalarProcedure("TOTAL_RECORD", DbPrcess);
            return DbResult;
        }

C.For Data Table
-	Create Stored Procedure
CREATE PROCEDURE GetDataPeopleInfo
AS
BEGIN
	SELECT Name, Address, PhoneNumber FROM [PeopleInfoTbl]
END

Using in Code 
using DBHelperForCore;
public datatable GetDataPeople()
        {
            DBParams DbPrcess = new DBParams();
            
            Datatable DbResult = ConnDB.connection. SelectProcedure("GetDataPeopleInfo", DbPrcess);
            if (DbResult.rows.count() > 1)
            {
                return DbResult;
            }
            else
            {
                return null;
            }
            
        }

