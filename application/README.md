## django
```python

```

## flask
```bash
$ pip install Flask
$ pip install Flask-SQLAlchemy
```
```python
import os
from flask import Flask, request, jsonify
from flask_sqlalchemy import SQLAlchemy

BASE_DIR = os.path.join(os.path.abspath(os.path.dirname(__file__)), 'data')
DB_PATH = os.path.join(BASE_DIR, 'database.db')
os.makedirs(BASE_DIR, exist_ok=True)

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = f'sqlite:///{DB_PATH}'
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False

db = SQLAlchemy(app)

class Table(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(80), unique=True, nullable=False)
    email = db.Column(db.String(120), unique=True, nullable=False)

    def serialize(self):
        return {"id": self.id, "username": self.username, "email": self.email}


with app.app_context():
    db.create_all()

    # DELETE
    db.session.query(Table).delete()
    db.session.commit()

    # INSERT
    db.session.add( Table(username='Alice', email='alice@example.com') )
    db.session.commit()

    # UPDATE
    user = Table.query.filter_by(username='Alice').first()
    user.email = 'alice_new@example.com'
    db.session.commit()

    # SELECT
    users = Table.query.all()
    user = Table.query.filter_by(username='Alice').first()
    print(user.serialize())

@app.route('/', methods=['GET'])
def index():
    users = Table.query.all()
    return jsonify([user.serialize() for user in users])

if __name__ == '__main__':
    app.run(debug=True, host='127.0.0.1', port=5000)
```


## pyqt5
```python
from PyQt5 import QtCore, QtGui, QtWidgets, QtSql

class Window(QtWidgets.QWidget):
    def __init__(self):
        super().__init__()
        
        # DATABASE
        db = QtSql.QSqlDatabase.addDatabase("QSQLITE")
        db.setDatabaseName(":memory:")
        db.open()

        # Query
        query = QtSql.QSqlQuery(db)
        query.exec_(
            """
            CREATE TABLE IF NOT EXISTS users (
                id INTEGER PRIMARY KEY AUTOINCREMENT,
                name TEXT,
                age INTEGER
            )
            """
        )
        query.exec_("DELETE FROM users")
        query.exec_("INSERT INTO users (name, age) VALUES ('Alice', 30)")
        query.exec_("INSERT INTO users (name, age) VALUES ('Bob', 25)")


        # MODEL
        model = QtSql.QSqlTableModel()
        model.setTable("users") # model.setQuery( QSqlQuery("SELECT * FROM users") )
        model.select()  # Automatically executes "SELECT * FROM users"

        # WIDGETS
        widget = QtWidgets.QTableView()
        widget.setModel(model)

        # LAYOUTS
        layout = QtWidgets.QVBoxLayout()
        layout.addWidget(widget)
        self.setLayout(layout)
        self.setGeometry(300, 300, 300, 200)

        db.close()
    
    def callback(self):
        pass

if __name__ == "__main__":
    import sys
    app = QtWidgets.QApplication(sys.argv)

    window = Window()
    window.show()
    sys.exit(app.exec_())
```

## Dash
```bash
$ pip install dash
$ pip install dash_table
```
```python
import dash
import dash_table
import pandas as pd
from dash import html

df = pd.DataFrame({
    "Name": ["Alice", "Bob", "Charlie"],
    "Age": [25, 30, 35],
    "City": ["New York", "Los Angeles", "Chicago"]
})

app = dash.Dash(__name__)
app.title = 'TITLE'
app.layout = html.Div([
    dash_table.DataTable(
        columns=[
            {"name": col, "id": col} for col in df.columns
        ],
        data=df.to_dict('records'),
        style_table={'height': '400px', 'overflowY': 'auto'},
        style_cell={'textAlign': 'center', 'padding': '10px'},
        style_header={'backgroundColor': 'lightblue', 'fontWeight': 'bold'},
    )
])

if __name__ == '__main__':
    app.run(debug=True, host="localhost", port=8050)
```


## Visdom
```bash
$ python -m visdom.server
$ python -m visdom.server -port 8097
$ python -m visdom.server --hostname localhost -port 8097
```
```python
import numpy as np
import pandas as pd
import visdom

vis = visdom.Visdom()
vis.check_connection()
vis.get_env_list()
vis.server, vis.port

# text
vis.text(
    pd.DataFrame(
        {
            "Column A": [1, 2, 3],
            "Column B": ["A", "B", "C"]
        }
    ).to_html()
)

vis.close()
```

## jupyter
### sqlite3
```bash
$ pip install sqlite3
```
```python
import sqlite3
import numpy as np
import pandas as pd

conn = sqlite3.connect(':memory:') # example.db
cursor = conn.cursor()

# INSERT
cursor.execute("""
    CREATE TABLE users (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        name TEXT,
        age INTEGER
    )
""")
cursor.execute("INSERT INTO users (name, age) VALUES (?, ?)", ("Alice", 25))
cursor.execute("INSERT INTO users (name, age) VALUES (?, ?)", ("Bob", 30))
conn.commit()

# SELECT
cursor.execute("SELECT * FROM users")
for row in cursor.fetchall():
    print(row)

cursor.close()
conn.close()
```
```python
import sqlite3
import numpy as np
import pandas as pd

conn = sqlite3.connect(':memory:') # example.db

# INSERT
pd.DataFrame(data=np.random.normal(size=(30, 5)), columns=list('ABCDE')).to_sql('RANDOM_TABLE', conn, index=False)

# SELECT
pd.read_sql("""select * from RANDOM_TABLE""", conn)

conn.close()
```

### mysql
```bash
$ pip install mysql-connector-python pymysql sqlalchemy
```

`mysql.connector`
```python
import mysql.connector

user = "root"        
password = "PASSWORD"  
host = "localhost" # "127.0.0.1"
port = 3306          

try:
    conn = mysql.connector.connect(
        host=host,
        user=user,
        password=password,
        port=port
    )
    print("Connection Success")
    
    cursor = conn.cursor()
    cursor.execute("""CREATE DATABASE IF NOT EXISTS example;""")
    cursor.execute("""USE example;""")
    cursor.execute("""
        CREATE TABLE IF NOT EXISTS users (
            id INT AUTO_INCREMENT PRIMARY KEY,
            name VARCHAR(255),
            age INT
        )
    """)
    cursor.execute("INSERT INTO users (name, age) VALUES (%s, %s)", ("Alice", 25))
    cursor.execute("INSERT INTO users (name, age) VALUES (%s, %s)", ("Bob", 30))
    conn.commit()

    cursor.execute("SELECT * FROM users")
    for row in cursor.fetchall():
        print(row)

    cursor.close()
    conn.close()

except mysql.connector.Error as e:
    print(f"Connection Fail: {e}")
```

`pymysql`
```python
import pymysql

user = "root"        
password = "PASSWORD"  
host = "localhost" # "127.0.0.1"
port = 3306          

try:
    conn = pymysql.connect(
        host=host,
        user=user,
        password=password,
        port=port
    )
    print("Connection Success")

    cursor = conn.cursor()
    cursor.execute("""CREATE DATABASE IF NOT EXISTS example;""")
    cursor.execute("""USE example;""")
    cursor.execute("""
        CREATE TABLE IF NOT EXISTS users (
            id INT AUTO_INCREMENT PRIMARY KEY,
            name VARCHAR(255),
            age INT
        )
    """)
    cursor.execute("INSERT INTO users (name, age) VALUES (%s, %s)", ("Alice", 25))
    cursor.execute("INSERT INTO users (name, age) VALUES (%s, %s)", ("Bob", 30))
    conn.commit()

    cursor.execute("SELECT * FROM users")
    for row in cursor.fetchall():
        print(row)

    cursor.close()
    conn.close()

except pymysql.MySQLError as e:
    print(f"Connection Fail: {e}")
```



`sqlalchemy`
```python
import numpy as np
import pandas as pd
from sqlalchemy import create_engine

user = "root"        
password = "PASSWORD"  
host = "localhost" # "127.0.0.1"
port = 3306          
dbname = "example"

try:
    engine = create_engine(f"mysql+pymysql://{user}:{password}@{host}:{port}/{dbname}")
    with engine.connect() as conn:
        print("Connection Success")
        
        pd.DataFrame(data=np.random.normal(size=(30, 5)), columns=list('ABCDE')).to_sql(name="RANDOM_TABLE", con=engine, if_exists=["replace", "append"][0], index=False)
        pd.read_sql("""select * from RANDOM_TABLE""", engine)

except Exception as e:
    print(f"Connection Fail: {e}")
```
