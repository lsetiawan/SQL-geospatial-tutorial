---
title: "Scripting database connections in Python/R"
teaching: 10
exercises: 20
questions:
objectives:
keypoints:
---


#### Interfacing with the database from Python:

We will use [pandas](pandas.pydata.org) which is a data analysis toolkit that happens to have some very simple methods for moving data to and from a database. The database connections are handled through pandas using [sqlalchemy](www.sqlalchemy.org).

```Python
import pandas as pd
from sqlalchemy import create_engine
import sqlalchemy.types as types
```
Our next step is to pass some database connection information to pandas/sqlalchemy so that we can establish a connection. We create a database "engine" object that is then used in subsequent operations as a portal to/from the database.

```Python
host = 'dssg2016.c5k9fqonks28.us-east-1.rds.amazonaws.com'
user = 'dssg_student'
password = 'dssg2016'
port = '5432'
dbname = 'dssg2016'
engine = create_engine('postgresql://' + user + ':' + password + '@' + host + ':' + port + '/' + dbname)
```
Now we can issue a query to the database as follows:

```Python
df =pd.read_sql('SELECT * FROM seattlecrimeincidents LIMIT 100', engine)
```

##### Connecting from a Jupyter Notebook

Within a Jupyter notebook we can actually directly type SQL queries in a cell using an ipython magic. For that we need to install the [ipython-sql](https://pypi.python.org/pypi/ipython-sql) package:

```
pip install ipython-sql
```


To test your installation, you can run all cells of the [TestConnection.ipynb](../code/TestConnection.ipynb) notebook. In the second cell you will have to substitute the word password with the real password for the database.

#### Interfacing with the database from R:

We will use the R package [PostgreSQL](https://cran.r-project.org/web/packages/RPostgreSQL/index.html).

```R
install.packages("PostgreSQL")
library(RPostgreSQL)
```

We can execute a query in the following way:

```R
drv <- dbDriver("PostgreSQL")
con <- d:Connect(drv, host="dssg2016.c5k9fqonks28.us-east-1.rds.amazonaws.com", user="dssg_student", password="dssg2016", dbname="dssg2016", port="5432")
rs <- dbSendQuery(con, "select * from seattlecrimeincidents limit 100"); 
df <- fetch(rs)
```

#### Useful Resources:
* fiddle with SQL: [sqlfiddle](http://sqlfiddle.com/)
* check out [SQLShare](https://sqlshare.escience.washington.edu/sqlshare/) - databases made easy (you can find SeattleCrimeIncidents and census datasets already there)
* manipulate `.csv` files locally with the SQL language: [csvkit](https://csvkit.readthedocs.io/en/1.0.2/)
* querying urban data sets with [Socrata Query Language (SoQL)](https://dev.socrata.com/docs/queries/)
