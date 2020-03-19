# Working with databases in python

## Virtual env

We need to download an external library to connect to mysql - to do this we need to use pip.

However downloading dependencies willy nilly can cause a few different issues later - as if we always refer to the most recent download, sometimes they come with non-backwards compatible issues. To get around this we create a virtual environment (virtualenv)

```bash
$ python -m venv my_virtual_env
```

You should notice a new folder is created for you called my_virtual_env

Next activate this virtual environment
```
$ . ./my_virtual_env/scripts/activate

```
The initial lone dot is needed. What this does, is instead of running the activate in a new session.  

To deactivate an environment, just run 
> deactivate

## Adding in mysql connector
Run the code:
> $ python3 -m pip install mysql-connector

> $ python3 -m pip install mysql-connector-python


## Creating a database
To create a database you will need to do the following:

```python
import mysql.connector as mysql

mydb = mysql.connect(
  host="localhost",
  user="root",
  passwd="root"
)

mycursor = mydb.cursor()

mycursor.execute("CREATE DATABASE my_database")
```
## Creating a Table
```python
import mysql.connector as mysql

mydb = mysql.connect(
  host="localhost",
  user="root",
  passwd="root",
  database="my_database"
)

mycursor = mydb.cursor()

mycursor.execute("CREATE TABLE customers (name VARCHAR(255), address VARCHAR(255))")
```

## Inserting a Row
```python
import mysql.connector as mysql

mydb = mysql.connect(
  host="localhost",
  user="root",
  passwd="root",
  database="my_database"
)

mycursor = mydb.cursor()

sql = "INSERT INTO customers (name, address) VALUES ('John', 'Highway 21')"
mycursor.execute(sql)

mydb.commit()

print(mycursor.rowcount, "record inserted.")
```

## Selecting from a table
```python
import mysql.connector as mysql

mydb = mysql.connect(
  host="localhost",
  user="root",
  passwd="root",
  database="my_database"
)

mycursor = mydb.cursor()

mycursor.execute("SELECT * FROM customers")

myresult = mycursor.fetchall()

for customer in myresult:
  print(customer)
```

Each customer is a tuple of information. These are similar to arrays in java.

