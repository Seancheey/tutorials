1: Python dependency installation
---
Since its about converting between DataFrame and SQL, of course we need to install both packages for DataFrame(pandas) and SQL(SQLAlchemy). 
```
pip3 install -U pandas sqlalchemy
```
SQLAlchemy is a SQL toolkit and Object Relational Mapper(ORM) that gives application developers the full power and flexibility of SQL. So in addition, depending on different SQL database you are using, you should also install different corresponding package. Here are few options you can choose from:

- PostgreSQL: `psycopg2` as default api. 
	- other: `psycopg2`, `pg8000`
- MySQL: `mysql-python` as default api.
	- other: `mysqldb`, `mysqlconnector`, `oursql`, `pymysql`
- Oracle: `cx_oracle` as default api.
- MS SQL Server: `pyodbc` as default api. 
	- other: `pymssql`
- SQLite: python built-in module as default api. 

Or you can go to [SQLAlchemy official site](http://docs.sqlalchemy.org/en/latest/core/engines.html) for more info about api choices.

2: Convert from SQL to DataFrame
---
Pandas provides 3 functions to read SQL content: `read_sql`, `read_sql_table` and `read_sql_query`, where `read_sql` is a convinent wrapper for the other two. So for the most of the time, we only uses `read_sql`, as depending on the provided sql input, it will delegate to the specific function for us. For more reference, check [pandas.read_sql](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.read_sql.html). 

function prototype:
```python
pandas.read_sql(sql, con, index_col=None, coerce_float=True, params=None, parse_dates=None, columns=None, chunksize=None)
```

The second parameter above `con` is a SQLAlchemy connectable. Pandas uses it to decide which database to connect and how to connect etc. We can create an connectable by `sqlalchemy.create_engine` function that accepts a string of connection configuration by format `dialect[+driver]://user:password@host/dbname`.

So to summarize, we read from SQL by following command:
```python
from sqlalchemy import create_engine
from pandas import read_sql
engine = create_engine('mysql+pymysql://root:123456@127.0.0.1:3306/my_db')
my_data = read_sql("my_table_name", engine)
my_data2 = read_sql("SELECT * FROM my_table_name", engine)
```

3: Convert from DataFrame to SQL
---

similarly, there is also a function called `to_sql` in DataFrame. 

function prototype:
```python
DataFrame.to_sql(name, con, schema=None, if_exists='fail', index=True, index_label=None, chunksize=None, dtype=None)
```
So basically, we do the almost same things here by first creating a connectable and then call `to_sql`.

```
from pandas import DataFrame
from sqlalchemy import create_engine
engine = create_engine('mysql+pymysql://root:123456@127.0.0.1:3306/my_db')
my_data = DataFrame([[1,2],[3,5]])
my_data.to_sql("my_table", engine)
```

... that easy!
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTM3MTczMTY5NF19
-->