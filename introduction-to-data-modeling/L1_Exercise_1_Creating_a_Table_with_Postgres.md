
# Lesson 1 Exercise 1: Creating a Table with PostgreSQL

### Walk through the basics of PostgreSQL. You will need to complete the following tasks:<li> Create a table in PostgreSQL, <li> Insert rows of data <li> Run a simple SQL query to validate the information. <br>
`#####` denotes where the code needs to be completed. 

#### Import the library 
*Note:* An error might popup after this command has executed. If it does, read it carefully before ignoring. 


```python
import psycopg2
```


```python
!echo "alter user student createdb;" | sudo -u postgres psql
```

    ALTER ROLE


### Create a connection to the database


```python
try: 
    conn = psycopg2.connect("host=127.0.0.1 dbname=studentdb user=student password=student")
except psycopg2.Error as e: 
    print("Error: Could not make connection to the Postgres database")
    print(e)
```

### Use the connection to get a cursor that can be used to execute queries.


```python
try: 
    cur = conn.cursor()
except psycopg2.Error as e: 
    print("Error: Could not get curser to the Database")
    print(e)
```

### TO-DO: Set automatic commit to be true so that each action is committed without having to call conn.commit() after each command. 


```python
# TO-DO: set automatic commit to be true
conn.set_session(autocommit=True)
```

### TO-DO: Create a database to do the work in. 


```python
## TO-DO: Add the database name within the CREATE DATABASE statement. You can choose your own db name.
try: 
    cur.execute("create database db_adrien")
except psycopg2.Error as e:
    print(e)
```

#### TO-DO: Add the database name in the connect statement. Let's close our connection to the default database, reconnect to the Udacity database, and get a new cursor.


```python
## TO-DO: Add the database name within the connect statement
try: 
    conn.close()
except psycopg2.Error as e:
    print(e)
    
try: 
    conn = psycopg2.connect("host=127.0.0.1 dbname=db_adrien user=student password=student")
except psycopg2.Error as e: 
    print("Error: Could not make connection to the Postgres database")
    print(e)
    
try: 
    cur = conn.cursor()
except psycopg2.Error as e: 
    print("Error: Could not get curser to the Database")
    print(e)

conn.set_session(autocommit=True)
```

### Create a Song Library that contains a list of songs, including the song name, artist name, year, album it was from, and if it was a single. 

`song_title
artist_name
year
album_name
single`



```python
## TO-DO: Finish writing the CREATE TABLE statement with the correct arguments
try: 
    cur.execute("CREATE TABLE IF NOT EXISTS SONG (id INT, song_title VARCHAR, artist_name VARCHAR, year VARCHAR, album_name VARCHAR, single BOOL);")
except psycopg2.Error as e: 
    print("Error: Issue creating table")
    print (e)
```

### TO-DO: Insert the following two rows in the table
`First Row:  "Across The Universe", "The Beatles", "1970", "Let It Be", "False"`

`Second Row: "Think For Yourself", "The Beatles", "1965", "Rubber Soul", "False"`


```python
## TO-DO: Finish the INSERT INTO statement with the correct arguments

try: 
    cur.execute("INSERT INTO SONG (song_title, artist_name, year, album_name, single) \
                 VALUES (%s, %s, %s, %s, %s)", \
                 ("Across The Universe", "The Beatles", "1970", "Let It Be", "False"))
except psycopg2.Error as e: 
    print("Error: Inserting Rows")
    print (e)
    
try: 
    cur.execute("INSERT INTO SONG (song_title, artist_name, year, album_name, single) \
                  VALUES (%s, %s, %s, %s, %s)",
                  ("Think For Yourself", "The Beatles", "1965", "Rubber Soul", "False"))
except psycopg2.Error as e: 
    print("Error: Inserting Rows")
    print (e)
```

### TO-DO: Validate your data was inserted into the table. 



```python
## TO-DO: Finish the SELECT * Statement 
try: 
    cur.execute("SELECT * FROM song;")
except psycopg2.Error as e: 
    print("Error: select *")
    print (e)

row = cur.fetchone()
while row:
   print(row)
   row = cur.fetchone()
```

    (None, 'Across The Universe', 'The Beatles', '1970', 'Let It Be', False)
    (None, 'Think For Yourself', 'The Beatles', '1965', 'Rubber Soul', False)


### And finally close your cursor and connection. 


```python
cur.close()
conn.close()
```
