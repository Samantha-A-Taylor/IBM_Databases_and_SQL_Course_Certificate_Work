<a class="anchor" id="BTT"></a>
***
# **<font color="#B56447">Data Analysis of Public School Data, Crime Data, and Socioeconomic Indicators in Chicago Community Areas</font>**

### **S. Taylor**
***
### Introduction
This document serves as the final project for the [IBM Databases and SQL for Data Science with Python Certification course](https://www.coursera.org/learn/sql-data-science).
***
### Table of Contents
* [Overview and Preparation of the Datasets](#first-bullet)
* [Analyzing the Datasets](#third-bullet)

***
## **<font color="#B56447">Overview and Preparation of the Datasets</font>** <a class="anchor" id="first-bullet"></a>

The city of Chicago released a dataset showing all school level performance data used to create School Report Cards for the 2011-2012 school year. The dataset is available from the <a href="https://data.cityofchicago.org/Education/Chicago-Public-Schools-Progress-Report-Cards-2011-/9xs2-f89t?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkDB0201ENSkillsNetwork20127838-2021-01-01&cm_mmc=Email_Newsletter-\_-Developer_Ed%2BTech-\_-WW_WW-\_-SkillsNetwork-Courses-IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork-20127838&cm_mmca1=000026UJ&cm_mmca2=10006555&cm_mmca3=M12345678&cvosrc=email.Newsletter.M12345678&cvo_campaign=000026UJ">Chicago Data Portal</a>.

To complete the assignment problems in this notebook, three datasets that are available on the city of Chicago's Data Portal will be utilized:

1. <a href="https://data.cityofchicago.org/Health-Human-Services/Census-Data-Selected-socioeconomic-indicators-in-C/kn9c-c2s2?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkDB0201ENSkillsNetwork20127838-2021-01-01">Socioeconomic Indicators in Chicago</a>
    <br>This dataset contains a selection of six socioeconomic indicators of public health significance and a “hardship index,” for each Chicago community area, for the years 2008 – 2012.


2. <a href="https://data.cityofchicago.org/Education/Chicago-Public-Schools-Progress-Report-Cards-2011-/9xs2-f89t?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkDB0201ENSkillsNetwork20127838-2021-01-01">Chicago Public Schools</a> <br>This dataset shows all school level performance data used to create CPS School Report Cards for the 2011-2012 school year. This dataset is provided by the city of Chicago's Data Portal.


3. <a href="https://data.cityofchicago.org/Public-Safety/Crimes-2001-to-present/ijzp-q8t2?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkDB0201ENSkillsNetwork20127838-2021-01-01">Chicago Crime Data</a> <br>This dataset reflects reported incidents of crime (with the exception of murders where data exists for each victim) that occurred in the City of Chicago from 2001 to present, minus the most recent seven days.

***
### **Downloading the Datasets**

Execute the below code cell to install the required libraries.

This assignment requires these three tables populated with a subset of the whole datasets.
* Chicago Census Data
* Chicago Public Schools
* Chicago Crime Data

Read the data files using the Pandas library.


```python
%%capture
!pip install pandas
!pip install ipython-sql prettytable 

import prettytable

prettytable.DEFAULT = 'DEFAULT'
```

### Store the datasets in database tables

To analyze the data using SQL, it first needs to be loaded into SQLite DB.
We will create three tables in as under:

1.  **CENSUS_DATA**
2.  **CHICAGO_PUBLIC_SCHOOLS**
3.  **CHICAGO_CRIME_DATA**


Load the `pandas` and `sqlite3` libraries and establish a connection to `FinalDB.db`



```python
#%%capture
import csv, sqlite3

con = sqlite3.connect("FinalDB.db")
cur = con.cursor()
```

Load the SQL magic module.


```python
%load_ext sql
```

Use `Pandas` to load the data available in the links above to dataframes. Use these dataframes to load data on to the database `FinalDB.db` as required tables.



```python
import pandas
Census_df = pandas.read_csv("https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork/labs/FinalModule_Coursera_V5/data/ChicagoCensusData.csv?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkDB0201ENSkillsNetwork20127838-2021-01-01")
Schools_df = pandas.read_csv("https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork/labs/FinalModule_Coursera_V5/data/ChicagoPublicSchools.csv?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkDB0201ENSkillsNetwork20127838-2021-01-01")
Crime_df = pandas.read_csv("https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork/labs/FinalModule_Coursera_V5/data/ChicagoCrimeData.csv?utm_medium=Exinfluencer&utm_source=Exinfluencer&utm_content=000026UJ&utm_term=10006555&utm_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkDB0201ENSkillsNetwork20127838-2021-01-01")
```

Establish a connection between SQL magic module and the database `FinalDB.db`



```python
%sql sqlite:///FinalDB.db
```


```python
%%capture
Census_df.to_sql("CENSUS_DATA", con, if_exists='replace', index=False, method="multi")
Schools_df.to_sql("CHICAGO_PUBLIC_SCHOOLS", con, if_exists='replace', index=False, method="multi")
Crime_df.to_sql("CHICAGO_CRIME_DATA", con, if_exists='replace', index=False, method="multi")
```

[Back to Top](#BTT)
***
## **<font color="#B56447">Analyzing the Datasets</font>** <a class="anchor" id="third-bullet"></a>

Now write and execute SQL queries to solve assignment problems.

### Problem 1

##### Find the total number of crimes recorded in the CRIME table.


```python
%sql SELECT COUNT(*) \
    FROM CHICAGO_CRIME_DATA;
```

     * sqlite:///FinalDB.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>COUNT(*)</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>533</td>
        </tr>
    </tbody>
</table>



### Problem 2

##### List community area names and numbers with per capita income less than 11000.



```python
%sql SELECT COMMUNITY_AREA_NAME, COMMUNITY_AREA_NUMBER \
    FROM CENSUS_DATA \
    WHERE PER_CAPITA_INCOME < 11000;
```

     * sqlite:///FinalDB.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>COMMUNITY_AREA_NAME</th>
            <th>COMMUNITY_AREA_NUMBER</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>West Garfield Park</td>
            <td>26.0</td>
        </tr>
        <tr>
            <td>South Lawndale</td>
            <td>30.0</td>
        </tr>
        <tr>
            <td>Fuller Park</td>
            <td>37.0</td>
        </tr>
        <tr>
            <td>Riverdale</td>
            <td>54.0</td>
        </tr>
    </tbody>
</table>



### Problem 3

##### List all case numbers for crimes involving minors? (children are not considered minors for the purposes of crime analysis) 



```python
%sql SELECT CASE_NUMBER FROM CHICAGO_CRIME_DATA \
    WHERE DESCRIPTION LIKE '%MINOR%';
```

     * sqlite:///FinalDB.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>CASE_NUMBER</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>HL266884</td>
        </tr>
        <tr>
            <td>HK238408</td>
        </tr>
    </tbody>
</table>



### Problem 4

##### List all kidnapping crimes involving a child?



```python
%sql SELECT * FROM CHICAGO_CRIME_DATA \
    WHERE PRIMARY_TYPE like '%KIDNAPPING%';
```

     * sqlite:///FinalDB.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>ID</th>
            <th>CASE_NUMBER</th>
            <th>DATE</th>
            <th>BLOCK</th>
            <th>IUCR</th>
            <th>PRIMARY_TYPE</th>
            <th>DESCRIPTION</th>
            <th>LOCATION_DESCRIPTION</th>
            <th>ARREST</th>
            <th>DOMESTIC</th>
            <th>BEAT</th>
            <th>DISTRICT</th>
            <th>WARD</th>
            <th>COMMUNITY_AREA_NUMBER</th>
            <th>FBICODE</th>
            <th>X_COORDINATE</th>
            <th>Y_COORDINATE</th>
            <th>YEAR</th>
            <th>LATITUDE</th>
            <th>LONGITUDE</th>
            <th>LOCATION</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>5276766</td>
            <td>HN144152</td>
            <td>2007-01-26</td>
            <td>050XX W VAN BUREN ST</td>
            <td>1792</td>
            <td>KIDNAPPING</td>
            <td>CHILD ABDUCTION/STRANGER</td>
            <td>STREET</td>
            <td>0</td>
            <td>0</td>
            <td>1533</td>
            <td>15</td>
            <td>29.0</td>
            <td>25.0</td>
            <td>20</td>
            <td>1143050.0</td>
            <td>1897546.0</td>
            <td>2007</td>
            <td>41.87490841</td>
            <td>-87.75024931</td>
            <td>(41.874908413, -87.750249307)</td>
        </tr>
    </tbody>
</table>




```python
%sql SELECT * FROM CHICAGO_CRIME_DATA \
    WHERE PRIMARY_TYPE like '%KIDNAPPING%' and description like '%child%';
```

     * sqlite:///FinalDB.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>ID</th>
            <th>CASE_NUMBER</th>
            <th>DATE</th>
            <th>BLOCK</th>
            <th>IUCR</th>
            <th>PRIMARY_TYPE</th>
            <th>DESCRIPTION</th>
            <th>LOCATION_DESCRIPTION</th>
            <th>ARREST</th>
            <th>DOMESTIC</th>
            <th>BEAT</th>
            <th>DISTRICT</th>
            <th>WARD</th>
            <th>COMMUNITY_AREA_NUMBER</th>
            <th>FBICODE</th>
            <th>X_COORDINATE</th>
            <th>Y_COORDINATE</th>
            <th>YEAR</th>
            <th>LATITUDE</th>
            <th>LONGITUDE</th>
            <th>LOCATION</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>5276766</td>
            <td>HN144152</td>
            <td>2007-01-26</td>
            <td>050XX W VAN BUREN ST</td>
            <td>1792</td>
            <td>KIDNAPPING</td>
            <td>CHILD ABDUCTION/STRANGER</td>
            <td>STREET</td>
            <td>0</td>
            <td>0</td>
            <td>1533</td>
            <td>15</td>
            <td>29.0</td>
            <td>25.0</td>
            <td>20</td>
            <td>1143050.0</td>
            <td>1897546.0</td>
            <td>2007</td>
            <td>41.87490841</td>
            <td>-87.75024931</td>
            <td>(41.874908413, -87.750249307)</td>
        </tr>
    </tbody>
</table>



### Problem 5

##### List the kind of crimes that were recorded at schools. (No repetitions)



```python
%sql SELECT DISTINCT PRIMARY_TYPE, DESCRIPTION FROM CHICAGO_CRIME_DATA \
    WHERE LOCATION_DESCRIPTION like '%SCHOOL%';
```

     * sqlite:///FinalDB.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>PRIMARY_TYPE</th>
            <th>DESCRIPTION</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>BATTERY</td>
            <td>SIMPLE</td>
        </tr>
        <tr>
            <td>BATTERY</td>
            <td>PRO EMP HANDS NO/MIN INJURY</td>
        </tr>
        <tr>
            <td>CRIMINAL DAMAGE</td>
            <td>TO VEHICLE</td>
        </tr>
        <tr>
            <td>NARCOTICS</td>
            <td>POSS: HEROIN(WHITE)</td>
        </tr>
        <tr>
            <td>NARCOTICS</td>
            <td>MANU/DEL:CANNABIS 10GM OR LESS</td>
        </tr>
        <tr>
            <td>ASSAULT</td>
            <td>PRO EMP HANDS NO/MIN INJURY</td>
        </tr>
        <tr>
            <td>CRIMINAL TRESPASS</td>
            <td>TO LAND</td>
        </tr>
        <tr>
            <td>PUBLIC PEACE VIOLATION</td>
            <td>BOMB THREAT</td>
        </tr>
    </tbody>
</table>



### Problem 6

##### List the type of schools along with the average safety score for each type.



```python
%%capture
%%sql
ALTER TABLE CHICAGO_PUBLIC_SCHOOLS
RENAME "Elementary, Middle, or High School" TO School_Type;
```


```python
%sql SELECT School_Type, AVG(SAFETY_SCORE) AS AVERAGE_SAFETY_SCORE \
    FROM CHICAGO_PUBLIC_SCHOOLS \
    GROUP BY School_Type;
```

     * sqlite:///FinalDB.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>School_Type</th>
            <th>AVERAGE_SAFETY_SCORE</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ES</td>
            <td>49.52038369304557</td>
        </tr>
        <tr>
            <td>HS</td>
            <td>49.62352941176471</td>
        </tr>
        <tr>
            <td>MS</td>
            <td>48.0</td>
        </tr>
    </tbody>
</table>



### Problem 7

##### List 5 community areas with highest % of households below poverty line



```python
%sql SELECT COMMUNITY_AREA_NAME, PERCENT_HOUSEHOLDS_BELOW_POVERTY \
    FROM CENSUS_DATA \
    ORDER BY PERCENT_HOUSEHOLDS_BELOW_POVERTY DESC \
    LIMIT 5;
```

     * sqlite:///FinalDB.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>COMMUNITY_AREA_NAME</th>
            <th>PERCENT_HOUSEHOLDS_BELOW_POVERTY</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Riverdale</td>
            <td>56.5</td>
        </tr>
        <tr>
            <td>Fuller Park</td>
            <td>51.2</td>
        </tr>
        <tr>
            <td>Englewood</td>
            <td>46.6</td>
        </tr>
        <tr>
            <td>North Lawndale</td>
            <td>43.1</td>
        </tr>
        <tr>
            <td>East Garfield Park</td>
            <td>42.4</td>
        </tr>
    </tbody>
</table>



### Problem 8

##### Which community area is most crime prone? Display the coumminty area number only.



```python
%sql SELECT COMMUNITY_AREA_NUMBER \
    FROM CHICAGO_CRIME_DATA \
    GROUP BY COMMUNITY_AREA_NUMBER \
    ORDER BY COUNT(COMMUNITY_AREA_NUMBER) DESC \
    LIMIT 1;
```

     * sqlite:///FinalDB.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>COMMUNITY_AREA_NUMBER</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>25.0</td>
        </tr>
    </tbody>
</table>




```python
%sql CREATE TABLE Problem08 AS \
    SELECT COMMUNITY_AREA_NUMBER, COUNT(COMMUNITY_AREA_NUMBER) AS CRIME_COUNT \
    FROM CHICAGO_CRIME_DATA \
    GROUP BY COMMUNITY_AREA_NUMBER \
    ORDER BY COUNT(COMMUNITY_AREA_NUMBER) DESC LIMIT 5;
```

     * sqlite:///FinalDB.db
    Done.
    




    []




```python
import matplotlib.pyplot as plt
%matplotlib inline
```


```python
Crime_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>ID</th>
      <th>CASE_NUMBER</th>
      <th>DATE</th>
      <th>BLOCK</th>
      <th>IUCR</th>
      <th>PRIMARY_TYPE</th>
      <th>DESCRIPTION</th>
      <th>LOCATION_DESCRIPTION</th>
      <th>ARREST</th>
      <th>DOMESTIC</th>
      <th>...</th>
      <th>DISTRICT</th>
      <th>WARD</th>
      <th>COMMUNITY_AREA_NUMBER</th>
      <th>FBICODE</th>
      <th>X_COORDINATE</th>
      <th>Y_COORDINATE</th>
      <th>YEAR</th>
      <th>LATITUDE</th>
      <th>LONGITUDE</th>
      <th>LOCATION</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>3512276</td>
      <td>HK587712</td>
      <td>2004-08-28</td>
      <td>047XX S KEDZIE AVE</td>
      <td>890</td>
      <td>THEFT</td>
      <td>FROM BUILDING</td>
      <td>SMALL RETAIL STORE</td>
      <td>False</td>
      <td>False</td>
      <td>...</td>
      <td>9</td>
      <td>14.0</td>
      <td>58.0</td>
      <td>6</td>
      <td>1155838.0</td>
      <td>1873050.0</td>
      <td>2004</td>
      <td>41.807440</td>
      <td>-87.703956</td>
      <td>(41.8074405, -87.703955849)</td>
    </tr>
    <tr>
      <th>1</th>
      <td>3406613</td>
      <td>HK456306</td>
      <td>2004-06-26</td>
      <td>009XX N CENTRAL PARK AVE</td>
      <td>820</td>
      <td>THEFT</td>
      <td>$500 AND UNDER</td>
      <td>OTHER</td>
      <td>False</td>
      <td>False</td>
      <td>...</td>
      <td>11</td>
      <td>27.0</td>
      <td>23.0</td>
      <td>6</td>
      <td>1152206.0</td>
      <td>1906127.0</td>
      <td>2004</td>
      <td>41.898280</td>
      <td>-87.716406</td>
      <td>(41.898279962, -87.716405505)</td>
    </tr>
    <tr>
      <th>2</th>
      <td>8002131</td>
      <td>HT233595</td>
      <td>2011-04-04</td>
      <td>043XX S WABASH AVE</td>
      <td>820</td>
      <td>THEFT</td>
      <td>$500 AND UNDER</td>
      <td>NURSING HOME/RETIREMENT HOME</td>
      <td>False</td>
      <td>False</td>
      <td>...</td>
      <td>2</td>
      <td>3.0</td>
      <td>38.0</td>
      <td>6</td>
      <td>1177436.0</td>
      <td>1876313.0</td>
      <td>2011</td>
      <td>41.815933</td>
      <td>-87.624642</td>
      <td>(41.815933131, -87.624642127)</td>
    </tr>
    <tr>
      <th>3</th>
      <td>7903289</td>
      <td>HT133522</td>
      <td>2010-12-30</td>
      <td>083XX S KINGSTON AVE</td>
      <td>840</td>
      <td>THEFT</td>
      <td>FINANCIAL ID THEFT: OVER $300</td>
      <td>RESIDENCE</td>
      <td>False</td>
      <td>False</td>
      <td>...</td>
      <td>4</td>
      <td>7.0</td>
      <td>46.0</td>
      <td>6</td>
      <td>1194622.0</td>
      <td>1850125.0</td>
      <td>2010</td>
      <td>41.743665</td>
      <td>-87.562463</td>
      <td>(41.743665322, -87.562462756)</td>
    </tr>
    <tr>
      <th>4</th>
      <td>10402076</td>
      <td>HZ138551</td>
      <td>2016-02-02</td>
      <td>033XX W 66TH ST</td>
      <td>820</td>
      <td>THEFT</td>
      <td>$500 AND UNDER</td>
      <td>ALLEY</td>
      <td>False</td>
      <td>False</td>
      <td>...</td>
      <td>8</td>
      <td>15.0</td>
      <td>66.0</td>
      <td>6</td>
      <td>1155240.0</td>
      <td>1860661.0</td>
      <td>2016</td>
      <td>41.773455</td>
      <td>-87.706480</td>
      <td>(41.773455295, -87.706480471)</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 21 columns</p>
</div>



### Problem 9

##### Use a sub-query to find the name of the community area with highest hardship index



```python
%sql SELECT COMMUNITY_AREA_NAME FROM CENSUS_DATA \
    WHERE hardship_index IN (SELECT MAX(hardship_index) FROM CENSUS_DATA)
```

     * sqlite:///FinalDB.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>COMMUNITY_AREA_NAME</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Riverdale</td>
        </tr>
    </tbody>
</table>



### Problem 10

##### Use a sub-query to determine the Community Area Name with most number of crimes?



```python
%sql SELECT COMMUNITY_AREA_NAME FROM CENSUS_DATA \
    WHERE COMMUNITY_AREA_NUMBER = (SELECT COMMUNITY_AREA_NUMBER \
                                       FROM CHICAGO_CRIME_DATA \
                                       GROUP BY COMMUNITY_AREA_NUMBER \
                                       ORDER BY COUNT(COMMUNITY_AREA_NUMBER) DESC \
                                       LIMIT 1);
```

     * sqlite:///FinalDB.db
    Done.
    




<table>
    <thead>
        <tr>
            <th>COMMUNITY_AREA_NAME</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Austin</td>
        </tr>
    </tbody>
</table>



[Back to Top](#BTT)

***
#### IBM Certificate Author(s)
Hima Vasudevan, Rav Ahuja, Ramesh Sannreddy

#### IBM Certificate Contribtuor(s)
Malika Singla, Abhishek Gagneja

#### <h3 align="center"> © IBM Corporation 2023. All rights reserved. <h3/>
