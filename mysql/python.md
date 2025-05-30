## Python

### MySQL
```bash
$ sudo mysql
$ sudo mysql -u root -p
```
```mysql
mysql> CREATE DATABASE testdb;
mysql> USE mysql;
mysql> ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'temppw';
mysql> FLUSH PRIVILEGES;
mysql> EXIT;
```

### PyMySQL: Raw Query

```bash
$ pip install pymysql sqlalchemy pandas
```

`PyMySQL`
```python
import pymysql

user = "root"        
password = "temppw"  
host = "localhost"   
port = 3306          

try:
    connection = pymysql.connect(
        host=host,
        user=user,
        password=password,
        port=port
    )
    print("Connection Success")

    cursor = connection.cursor()    
    cursor.execute("""CREATE DATABASE IF NOT EXISTS testdb;""")
    
    cursor.close()
    connection.close()
except pymysql.MySQLError as e:
    print(f"Connection Fail: {e}")
```


### SQLAlchemy

`SQLAlchemy`
```python
from sqlalchemy import create_engine, text

user = "root"        
password = "temppw"  
host = "localhost"   
port = 3306          
dbname = "testdb"

try:
    engine = create_engine(f"mysql+pymysql://{user}:{password}@{host}:{port}/{dbname}")
    with engine.connect() as conn:
        conn.execute(text("""
            CREATE TABLE IF NOT EXISTS testtable (
                id INT AUTO_INCREMENT PRIMARY KEY,
                name VARCHAR(100),
                created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
            )
        """))

        print("Connection Success")
except Exception as e:
    print(f"Connection Fail: {e}")
```


`SQLAlchemy with Pandas`
```python
import numpy as np
import pandas as pd
from sqlalchemy import create_engine

user = "root"        
password = "temppw"  
host = "localhost"   
port = 3306          
dbname = "testdb"

try:
    engine = create_engine(f"mysql+pymysql://{user}:{password}@{host}:{port}/{dbname}")
    with engine.connect() as conn:
        print("Connection Success")
        
        pd.DataFrame(data=np.random.normal(size=(30, 5)), columns=list('ABCDE')).to_sql(name="testtable", con=engine, if_exists=["replace", "append"][0], index=False)
        pd.read_sql("""select * from testtable""", engine)

except Exception as e:
    print(f"Connection Fail: {e}")
```



```python
from sqlalchemy import create_engine, text
import numpy as np
import pandas as pd

# DB INFO
user = "root"        
password = "temppw"  
host = "localhost"   
port = 3306          

engine = create_engine(f"mysql+pymysql://{user}:{password}@{host}:{port}")
with engine.connect() as conn:
    dbname = "testenv"
    conn.execute(text(f"CREATE DATABASE IF NOT EXISTS {dbname}"))

subengine = create_engine(f"mysql+pymysql://{user}:{password}@{host}:{port}/{dbname}")
with subengine.connect() as conn:
    tblname = "sampletbl"
    df = pd.DataFrame(data=np.random.normal(size=(30, 5)), columns=list('ABCDE'))
    df.to_sql(name=tblname, con=subengine, if_exists=["replace", "append"][0], index=False)
pd.read_sql(f"""SELECT * FROM {tblname}""", subengine)
```
