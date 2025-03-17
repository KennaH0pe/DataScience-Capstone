<p style="text-align:center">
    <a href="https://skills.network" target="_blank">
    <img src="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/assets/logos/SN_web_lightmode.png" width="200" alt="Skills Network Logo">
    </a>
</p>

<h1 align=center><font size = 5>Assignment: SQL Notebook for Peer Assignment</font></h1>

Estimated time needed: **60** minutes.

## Introduction
Using this Python notebook you will:

1.  Understand the Spacex DataSet
2.  Load the dataset  into the corresponding table in a Db2 database
3.  Execute SQL queries to answer assignment questions 


## Overview of the DataSet

SpaceX has gained worldwide attention for a series of historic milestones. 

It is the only private company ever to return a spacecraft from low-earth orbit, which it first accomplished in December 2010.
SpaceX advertises Falcon 9 rocket launches on its website with a cost of 62 million dollars wheras other providers cost upward of 165 million dollars each, much of the savings is because Space X can reuse the first stage. 


Therefore if we can determine if the first stage will land, we can determine the cost of a launch. 

This information can be used if an alternate company wants to bid against SpaceX for a rocket launch.

This dataset includes a record for each payload carried during a SpaceX mission into outer space.


### Download the datasets

This assignment requires you to load the spacex dataset.

In many cases the dataset to be analyzed is available as a .CSV (comma separated values) file, perhaps on the internet. Click on the link below to download and save the dataset (.CSV file):

 <a href="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-DS0321EN-SkillsNetwork/labs/module_2/data/Spacex.csv" target="_blank">Spacex DataSet</a>




```python
!pip install sqlalchemy>=1.4.1

```

### Connect to the database

Let us first load the SQL extension and establish a connection with the database



```python
!pip install ipython-sql
!pip install ipython-sql prettytable
```

    Collecting ipython-sql
      Downloading ipython_sql-0.5.0-py3-none-any.whl.metadata (17 kB)
    Collecting prettytable (from ipython-sql)
      Downloading prettytable-3.15.1-py3-none-any.whl.metadata (33 kB)
    Requirement already satisfied: ipython in /opt/conda/lib/python3.12/site-packages (from ipython-sql) (8.31.0)
    Requirement already satisfied: sqlalchemy>=2.0 in /opt/conda/lib/python3.12/site-packages (from ipython-sql) (2.0.37)
    Collecting sqlparse (from ipython-sql)
      Downloading sqlparse-0.5.3-py3-none-any.whl.metadata (3.9 kB)
    Requirement already satisfied: six in /opt/conda/lib/python3.12/site-packages (from ipython-sql) (1.17.0)
    Requirement already satisfied: ipython-genutils in /opt/conda/lib/python3.12/site-packages (from ipython-sql) (0.2.0)
    Requirement already satisfied: greenlet!=0.4.17 in /opt/conda/lib/python3.12/site-packages (from sqlalchemy>=2.0->ipython-sql) (3.1.1)
    Requirement already satisfied: typing-extensions>=4.6.0 in /opt/conda/lib/python3.12/site-packages (from sqlalchemy>=2.0->ipython-sql) (4.12.2)
    Requirement already satisfied: decorator in /opt/conda/lib/python3.12/site-packages (from ipython->ipython-sql) (5.1.1)
    Requirement already satisfied: jedi>=0.16 in /opt/conda/lib/python3.12/site-packages (from ipython->ipython-sql) (0.19.2)
    Requirement already satisfied: matplotlib-inline in /opt/conda/lib/python3.12/site-packages (from ipython->ipython-sql) (0.1.7)
    Requirement already satisfied: pexpect>4.3 in /opt/conda/lib/python3.12/site-packages (from ipython->ipython-sql) (4.9.0)
    Requirement already satisfied: prompt_toolkit<3.1.0,>=3.0.41 in /opt/conda/lib/python3.12/site-packages (from ipython->ipython-sql) (3.0.50)
    Requirement already satisfied: pygments>=2.4.0 in /opt/conda/lib/python3.12/site-packages (from ipython->ipython-sql) (2.19.1)
    Requirement already satisfied: stack_data in /opt/conda/lib/python3.12/site-packages (from ipython->ipython-sql) (0.6.3)
    Requirement already satisfied: traitlets>=5.13.0 in /opt/conda/lib/python3.12/site-packages (from ipython->ipython-sql) (5.14.3)
    Requirement already satisfied: wcwidth in /opt/conda/lib/python3.12/site-packages (from prettytable->ipython-sql) (0.2.13)
    Requirement already satisfied: parso<0.9.0,>=0.8.4 in /opt/conda/lib/python3.12/site-packages (from jedi>=0.16->ipython->ipython-sql) (0.8.4)
    Requirement already satisfied: ptyprocess>=0.5 in /opt/conda/lib/python3.12/site-packages (from pexpect>4.3->ipython->ipython-sql) (0.7.0)
    Requirement already satisfied: executing>=1.2.0 in /opt/conda/lib/python3.12/site-packages (from stack_data->ipython->ipython-sql) (2.1.0)
    Requirement already satisfied: asttokens>=2.1.0 in /opt/conda/lib/python3.12/site-packages (from stack_data->ipython->ipython-sql) (3.0.0)
    Requirement already satisfied: pure_eval in /opt/conda/lib/python3.12/site-packages (from stack_data->ipython->ipython-sql) (0.2.3)
    Downloading ipython_sql-0.5.0-py3-none-any.whl (20 kB)
    Downloading prettytable-3.15.1-py3-none-any.whl (33 kB)
    Downloading sqlparse-0.5.3-py3-none-any.whl (44 kB)
    Installing collected packages: sqlparse, prettytable, ipython-sql
    Successfully installed ipython-sql-0.5.0 prettytable-3.15.1 sqlparse-0.5.3
    Requirement already satisfied: ipython-sql in /opt/conda/lib/python3.12/site-packages (0.5.0)
    Requirement already satisfied: prettytable in /opt/conda/lib/python3.12/site-packages (3.15.1)
    Requirement already satisfied: ipython in /opt/conda/lib/python3.12/site-packages (from ipython-sql) (8.31.0)
    Requirement already satisfied: sqlalchemy>=2.0 in /opt/conda/lib/python3.12/site-packages (from ipython-sql) (2.0.37)
    Requirement already satisfied: sqlparse in /opt/conda/lib/python3.12/site-packages (from ipython-sql) (0.5.3)
    Requirement already satisfied: six in /opt/conda/lib/python3.12/site-packages (from ipython-sql) (1.17.0)
    Requirement already satisfied: ipython-genutils in /opt/conda/lib/python3.12/site-packages (from ipython-sql) (0.2.0)
    Requirement already satisfied: wcwidth in /opt/conda/lib/python3.12/site-packages (from prettytable) (0.2.13)
    Requirement already satisfied: greenlet!=0.4.17 in /opt/conda/lib/python3.12/site-packages (from sqlalchemy>=2.0->ipython-sql) (3.1.1)
    Requirement already satisfied: typing-extensions>=4.6.0 in /opt/conda/lib/python3.12/site-packages (from sqlalchemy>=2.0->ipython-sql) (4.12.2)
    Requirement already satisfied: decorator in /opt/conda/lib/python3.12/site-packages (from ipython->ipython-sql) (5.1.1)
    Requirement already satisfied: jedi>=0.16 in /opt/conda/lib/python3.12/site-packages (from ipython->ipython-sql) (0.19.2)
    Requirement already satisfied: matplotlib-inline in /opt/conda/lib/python3.12/site-packages (from ipython->ipython-sql) (0.1.7)
    Requirement already satisfied: pexpect>4.3 in /opt/conda/lib/python3.12/site-packages (from ipython->ipython-sql) (4.9.0)
    Requirement already satisfied: prompt_toolkit<3.1.0,>=3.0.41 in /opt/conda/lib/python3.12/site-packages (from ipython->ipython-sql) (3.0.50)
    Requirement already satisfied: pygments>=2.4.0 in /opt/conda/lib/python3.12/site-packages (from ipython->ipython-sql) (2.19.1)
    Requirement already satisfied: stack_data in /opt/conda/lib/python3.12/site-packages (from ipython->ipython-sql) (0.6.3)
    Requirement already satisfied: traitlets>=5.13.0 in /opt/conda/lib/python3.12/site-packages (from ipython->ipython-sql) (5.14.3)
    Requirement already satisfied: parso<0.9.0,>=0.8.4 in /opt/conda/lib/python3.12/site-packages (from jedi>=0.16->ipython->ipython-sql) (0.8.4)
    Requirement already satisfied: ptyprocess>=0.5 in /opt/conda/lib/python3.12/site-packages (from pexpect>4.3->ipython->ipython-sql) (0.7.0)
    Requirement already satisfied: executing>=1.2.0 in /opt/conda/lib/python3.12/site-packages (from stack_data->ipython->ipython-sql) (2.1.0)
    Requirement already satisfied: asttokens>=2.1.0 in /opt/conda/lib/python3.12/site-packages (from stack_data->ipython->ipython-sql) (3.0.0)
    Requirement already satisfied: pure_eval in /opt/conda/lib/python3.12/site-packages (from stack_data->ipython->ipython-sql) (0.2.3)



```python
%load_ext sql
```


```python
import csv, sqlite3
import prettytable
prettytable.DEFAULT = 'DEFAULT'

con = sqlite3.connect("my_data1.db")
cur = con.cursor()
```


```python
!pip install -q pandas
```


```python
%sql sqlite:///my_data1.db
```


```python
import pandas as pd
df = pd.read_csv("https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-DS0321EN-SkillsNetwork/labs/module_2/data/Spacex.csv")
df.to_sql("SPACEXTBL", con, if_exists='replace', index=False,method="multi")
```




    101



**Note:This below code is added to remove blank rows from table**



```python
#DROP THE TABLE IF EXISTS

%sql DROP TABLE IF EXISTS SPACEXTABLE;
```

     * sqlite:///my_data1.db
    Done.





    []




```python
%sql create table SPACEXTABLE as select * from SPACEXTBL where Date is not null
```

     * sqlite:///my_data1.db
    Done.





    []



## Tasks

Now write and execute SQL queries to solve the assignment tasks.

**Note: If the column names are in mixed case enclose it in double quotes
   For Example "Landing_Outcome"**

### Task 1




##### Display the names of the unique launch sites  in the space mission



```python
%sql SELECT DISTINCT Launch_Site FROM SPACEXTBL
```

     * sqlite:///my_data1.db
    Done.





<table>
    <thead>
        <tr>
            <th>Launch_Site</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>CCAFS LC-40</td>
        </tr>
        <tr>
            <td>VAFB SLC-4E</td>
        </tr>
        <tr>
            <td>KSC LC-39A</td>
        </tr>
        <tr>
            <td>CCAFS SLC-40</td>
        </tr>
    </tbody>
</table>




### Task 2


#####  Display 5 records where launch sites begin with the string 'CCA' 



```python
%sql SELECT * FROM SPACEXTBL WHERE Launch_site LIKE 'CCA%' LIMIT 5;
```

     * sqlite:///my_data1.db
    Done.





<table>
    <thead>
        <tr>
            <th>Date</th>
            <th>Time (UTC)</th>
            <th>Booster_Version</th>
            <th>Launch_Site</th>
            <th>Payload</th>
            <th>PAYLOAD_MASS__KG_</th>
            <th>Orbit</th>
            <th>Customer</th>
            <th>Mission_Outcome</th>
            <th>Landing_Outcome</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>2010-06-04</td>
            <td>18:45:00</td>
            <td>F9 v1.0  B0003</td>
            <td>CCAFS LC-40</td>
            <td>Dragon Spacecraft Qualification Unit</td>
            <td>0</td>
            <td>LEO</td>
            <td>SpaceX</td>
            <td>Success</td>
            <td>Failure (parachute)</td>
        </tr>
        <tr>
            <td>2010-12-08</td>
            <td>15:43:00</td>
            <td>F9 v1.0  B0004</td>
            <td>CCAFS LC-40</td>
            <td>Dragon demo flight C1, two CubeSats, barrel of Brouere cheese</td>
            <td>0</td>
            <td>LEO (ISS)</td>
            <td>NASA (COTS) NRO</td>
            <td>Success</td>
            <td>Failure (parachute)</td>
        </tr>
        <tr>
            <td>2012-05-22</td>
            <td>7:44:00</td>
            <td>F9 v1.0  B0005</td>
            <td>CCAFS LC-40</td>
            <td>Dragon demo flight C2</td>
            <td>525</td>
            <td>LEO (ISS)</td>
            <td>NASA (COTS)</td>
            <td>Success</td>
            <td>No attempt</td>
        </tr>
        <tr>
            <td>2012-10-08</td>
            <td>0:35:00</td>
            <td>F9 v1.0  B0006</td>
            <td>CCAFS LC-40</td>
            <td>SpaceX CRS-1</td>
            <td>500</td>
            <td>LEO (ISS)</td>
            <td>NASA (CRS)</td>
            <td>Success</td>
            <td>No attempt</td>
        </tr>
        <tr>
            <td>2013-03-01</td>
            <td>15:10:00</td>
            <td>F9 v1.0  B0007</td>
            <td>CCAFS LC-40</td>
            <td>SpaceX CRS-2</td>
            <td>677</td>
            <td>LEO (ISS)</td>
            <td>NASA (CRS)</td>
            <td>Success</td>
            <td>No attempt</td>
        </tr>
    </tbody>
</table>



### Task 3




##### Display the total payload mass carried by boosters launched by NASA (CRS)



```python
%sql SELECT SUM(payload) AS Total_payload FROM SPACEXTBL WHERE Booster_Version LIKE 'NASA(CRS)%';
```

     * sqlite:///my_data1.db
    Done.





<table>
    <thead>
        <tr>
            <th>Total_payload</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>None</td>
        </tr>
    </tbody>
</table>



### Task 4




##### Display average payload mass carried by booster version F9 v1.1



```python
%sql SELECT AVG(payload) AS Average_payload FROM SPACEXTBL WHERE Booster_Version = 'F9 v1.1';
```

     * sqlite:///my_data1.db
    Done.





<table>
    <thead>
        <tr>
            <th>Average_payload</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>0.0</td>
        </tr>
    </tbody>
</table>



### Task 5

##### List the date when the first succesful landing outcome in ground pad was acheived.


_Hint:Use min function_ 



```python
%sql SELECT MIN(Date) AS First_Successful_Landing FROM SPACEXTBL WHERE Landing_Outcome = 'Success (ground pad)';
```

     * sqlite:///my_data1.db
    Done.





<table>
    <thead>
        <tr>
            <th>First_Successful_Landing</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>2015-12-22</td>
        </tr>
    </tbody>
</table>



### Task 6

##### List the names of the boosters which have success in drone ship and have payload mass greater than 4000 but less than 6000



```python
%sql SELECT Booster_Version FROM SPACEXTBL WHERE Landing_Outcome = 'Success (drone ship)' AND payload BETWEEN 4000 AND 6000;
```

     * sqlite:///my_data1.db
    Done.





<table>
    <thead>
        <tr>
            <th>Booster_Version</th>
        </tr>
    </thead>
    <tbody>
    </tbody>
</table>



### Task 7




##### List the total number of successful and failure mission outcomes



```python
%sql SELECT  Mission_Outcome, COUNT(*) AS Total_count FROM SPACEXTBL GROUP BY Mission_Outcome;
```

     * sqlite:///my_data1.db
    Done.





<table>
    <thead>
        <tr>
            <th>Mission_Outcome</th>
            <th>Total_count</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Failure (in flight)</td>
            <td>1</td>
        </tr>
        <tr>
            <td>Success</td>
            <td>98</td>
        </tr>
        <tr>
            <td>Success </td>
            <td>1</td>
        </tr>
        <tr>
            <td>Success (payload status unclear)</td>
            <td>1</td>
        </tr>
    </tbody>
</table>



### Task 8



##### List the   names of the booster_versions which have carried the maximum payload mass. Use a subquery



```python
%sql SELECT Booster_Version FROM SPACEXTBL WHERE payload =(SELECT MAX(payload) FROM SPACEXTBL);
```

     * sqlite:///my_data1.db
    Done.





<table>
    <thead>
        <tr>
            <th>Booster_Version</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>F9 B4 B1043.1</td>
        </tr>
    </tbody>
</table>



### Task 9


##### List the records which will display the month names, failure landing_outcomes in drone ship ,booster versions, launch_site for the months in year 2015.

**Note: SQLLite does not support monthnames. So you need to use  substr(Date, 6,2) as month to get the months and substr(Date,0,5)='2015' for year.**



```python
%sql SELECT CASE substr(Date, 6, 2) WHEN '01' THEN 'January' WHEN '02' THEN 'February' WHEN '03' THEN 'March' WHEN '04' THEN 'April' WHEN '05' THEN 'May' WHEN '06' THEN 'June' WHEN '07' THEN 'July' WHEN '08' THEN 'August' WHEN '09' THEN 'September' WHEN '10' THEN 'October' WHEN '11' THEN 'November' WHEN '12' THEN 'December' END AS Month_Name,Landing_Outcome,Booster_Version,Launch_Site FROM SPACEXTBL WHERE substr(Date, 1, 4) = '2015' AND Landing_Outcome LIKE '%Failure (drone ship)%';  

```

     * sqlite:///my_data1.db
    Done.





<table>
    <thead>
        <tr>
            <th>Month_Name</th>
            <th>Landing_Outcome</th>
            <th>Booster_Version</th>
            <th>Launch_Site</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>January</td>
            <td>Failure (drone ship)</td>
            <td>F9 v1.1 B1012</td>
            <td>CCAFS LC-40</td>
        </tr>
        <tr>
            <td>April</td>
            <td>Failure (drone ship)</td>
            <td>F9 v1.1 B1015</td>
            <td>CCAFS LC-40</td>
        </tr>
    </tbody>
</table>



### Task 10




##### Rank the count of landing outcomes (such as Failure (drone ship) or Success (ground pad)) between the date 2010-06-04 and 2017-03-20, in descending order.



```python
%sql SELECT Landing_Outcome,COUNT(*) AS Outcome_Count FROM SPACEXTBL WHERE Date BETWEEN '2010-06-04' AND '2017-03-20'AND (Landing_Outcome LIKE '%Failure (drone ship)%' OR Landing_Outcome LIKE '%Success (ground pad)%') GROUP BY  Landing_Outcome ORDER BY Outcome_Count DESC; 
```

     * sqlite:///my_data1.db
    Done.





<table>
    <thead>
        <tr>
            <th>Landing_Outcome</th>
            <th>Outcome_Count</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Failure (drone ship)</td>
            <td>5</td>
        </tr>
        <tr>
            <td>Success (ground pad)</td>
            <td>3</td>
        </tr>
    </tbody>
</table>



### Reference Links

* <a href ="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork/labs/Labs_Coursera_V5/labs/Lab%20-%20String%20Patterns%20-%20Sorting%20-%20Grouping/instructional-labs.md.html?origin=www.coursera.org">Hands-on Lab : String Patterns, Sorting and Grouping</a>  

*  <a  href="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork/labs/Labs_Coursera_V5/labs/Lab%20-%20Built-in%20functions%20/Hands-on_Lab__Built-in_Functions.md.html?origin=www.coursera.org">Hands-on Lab: Built-in functions</a>

*  <a  href="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork/labs/Labs_Coursera_V5/labs/Lab%20-%20Sub-queries%20and%20Nested%20SELECTs%20/instructional-labs.md.html?origin=www.coursera.org">Hands-on Lab : Sub-queries and Nested SELECT Statements</a>

*   <a href="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork/labs/Module%205/DB0201EN-Week3-1-3-SQLmagic.ipynb">Hands-on Tutorial: Accessing Databases with SQL magic</a>

*  <a href= "https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork/labs/Module%205/DB0201EN-Week3-1-4-Analyzing.ipynb">Hands-on Lab: Analyzing a real World Data Set</a>




## Author(s)

<h4> Lakshmi Holla </h4>


## Other Contributors

<h4> Rav Ahuja </h4>


<!--
## Change log
| Date | Version | Changed by | Change Description |
|------|--------|--------|---------|
| 2024-07-10 | 1.1 |Anita Verma | Changed Version|
| 2021-07-09 | 0.2 |Lakshmi Holla | Changes made in magic sql|
| 2021-05-20 | 0.1 |Lakshmi Holla | Created Initial Version |
-->


## <h3 align="center"> © IBM Corporation 2021. All rights reserved. <h3/>

