# python-sqlite3
`query:syntax`
```
# SYNTAX ORDER
SELECT * FROM * LEFT JOIN * ON * WHERE * GROUP BY * HAVING * ORDER BY *

# EXECUTION ORDER
FROM > ON > JOIN > WHERE > GROUP BY > HAVING > SELECT > DISTINCT > ORDER BY
```

## Database
```bash
$ sqlite3 DatabaseName.db
```
```sqlite
sqlite3> ATTACH DATABASE 'DatabaseName' As 'Alias-Name';
sqlite3> DETACH DATABASE 'Alias-Name';
```

### Table
```sqlite
sqlite3> .databases
sqlite3> .tables
sqlite3> .schema
sqlite3> .quit
```

```
sqlite3> DROP TABLE database_name.table_name;
sqlite3> ALTER TABLE database_name.table_name RENAME TO new_table_name;
sqlite3> ALTER TABLE database_name.table_name ADD COLUMN column_def...;
sqlite3> INSERT INTO table_name VALUES (value1, value2, value3, ... , valueN);
sqlite3> INSERT INTO table_name (column1, column2, column3, ... , columnN) VALUES (value1, value2, value3, ... , valueN);
sqlite3> INSERT INTO table_name (column1, column2, column3, ... , columnN) VALUES (value11, value12, value13, ... , value1N), (value21, value22, value23, ... , value2N), (valueM1, valueM2, valueM3, ... , valueMN);
sqlite3> SELECT column1, column2, ... , columnN FROM table_name;
```

## Python
```python
import sqlite3

conn = sqlite3.connect(':memory:')
conn.execute("""
    CREATE TABLE COMPANY
        (ID INT PRIMARY KEY NOT NULL
        , NAME TEXT NOT NULL
        , AGE INT NOT NULL
        , ADDRESS CHAR(50)
        , SALARY REAL);
""")
conn.close()
```

### Pandas
`:memory:`: `sqlite_master`
```python
import sqlite3
import numpy as np
import pandas as pd

conn = sqlite3.connect(':memory:')
pd.DataFrame(np.random.randint(0, 5, size=(30, 5)), columns=list('ABCDE')).to_sql('TABLE01', conn, index=False)
pd.DataFrame(np.random.randint(0, 5, size=(30, 5)), columns=list('ABCDE')).to_sql('TABLE02', conn, index=False)
pd.DataFrame(np.random.randint(0, 5, size=(30, 5)), columns=list('ABCDE')).map(lambda x: dict(enumerate(['a', 'b', 'c', 'd', 'e']))[x]).to_sql('TABLE03', conn, index=False)
pd.DataFrame(np.random.randint(0, 5, size=(30, 5)), columns=list('ABCDE')).map(lambda x: dict(enumerate(['a', 'b', 'c', 'd', 'e']))[x]).to_sql('TABLE04', conn, index=False)
pd.DataFrame(np.random.normal(size=(30, 5)), columns=list('ABCDE')).to_sql('TABLE05', conn, index=False)
display(pd.read_sql("""select * from sqlite_master""", conn)); conn.close()
```


```python
import sqlite3
import numpy as np
import pandas as pd

conn = sqlite3.connect('random.db')
pd.DataFrame(data=np.random.normal(size=(30, 5)), columns=list('ABCDE')).to_sql('RANDOM_TABLE', conn, index=False)
conn.close()

conn = sqlite3.connect('random.db')
pd.read_sql("""select * from RANDOM_TABLE""", conn)
conn.close()
```

`databases.db`
```python
import sqlite3
import numpy as np
import pandas as pd

conn = sqlite3.connect('databases.db')
conn.close()

conn = sqlite3.connect('database01.db')
pd.DataFrame(np.random.randint(0, 5, size=(30, 5)), columns=list('ABCDE')).map(lambda x: dict(enumerate(['a', 'b', 'c', 'd', 'e']))[x]).to_sql('TABLE01', conn, index=False)
pd.DataFrame(np.random.randint(0, 5, size=(30, 5)), columns=list('ABCDE')).map(lambda x: dict(enumerate(['a', 'b', 'c', 'd', 'e']))[x]).to_sql('TABLE02', conn, index=False)
pd.DataFrame(np.random.randint(0, 5, size=(30, 5)), columns=list('ABCDE')).map(lambda x: dict(enumerate(['a', 'b', 'c', 'd', 'e']))[x]).to_sql('TABLE03', conn, index=False)
conn.close()

conn = sqlite3.connect('database02.db')
pd.DataFrame(np.random.randint(0, 5, size=(30, 5)), columns=list('ABCDE')).map(lambda x: dict(enumerate(['a', 'b', 'c', 'd', 'e']))[x]).to_sql('TABLE04', conn, index=False)
pd.DataFrame(np.random.randint(0, 5, size=(30, 5)), columns=list('ABCDE')).map(lambda x: dict(enumerate(['a', 'b', 'c', 'd', 'e']))[x]).to_sql('TABLE05', conn, index=False)
pd.DataFrame(np.random.randint(0, 5, size=(30, 5)), columns=list('ABCDE')).map(lambda x: dict(enumerate(['a', 'b', 'c', 'd', 'e']))[x]).to_sql('TABLE06', conn, index=False)
conn.close()

conn = sqlite3.connect('database03.db')
pd.DataFrame(np.random.randint(0, 5, size=(30, 5)), columns=list('ABCDE')).map(lambda x: dict(enumerate(['a', 'b', 'c', 'd', 'e']))[x]).to_sql('TABLE07', conn, index=False)
pd.DataFrame(np.random.randint(0, 5, size=(30, 5)), columns=list('ABCDE')).map(lambda x: dict(enumerate(['a', 'b', 'c', 'd', 'e']))[x]).to_sql('TABLE08', conn, index=False)
pd.DataFrame(np.random.randint(0, 5, size=(30, 5)), columns=list('ABCDE')).map(lambda x: dict(enumerate(['a', 'b', 'c', 'd', 'e']))[x]).to_sql('TABLE09', conn, index=False)
conn.close()

conn = sqlite3.connect('databases.db')
conn.execute('ATTACH DATABASE "database01.db" AS db1')
conn.execute('ATTACH DATABASE "database02.db" AS db2')
conn.execute('ATTACH DATABASE "database03.db" AS db3')
pd.read_sql('select * from db1.sqlite_master', conn)
pd.read_sql('select * from db2.sqlite_master', conn)
pd.read_sql('select * from db3.sqlite_master', conn)
conn.close()
```

