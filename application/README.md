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
    app.run(debug=True)
```

## jupyter
`sqlite3`
```bash
$ pip install sqlite3
```
```python
import sqlite3
import numpy as np
import pandas as pd

conn = sqlite3.connect(':memory:') # example.db
pd.DataFrame(data=np.random.normal(size=(30, 5)), columns=list('ABCDE')).to_sql('RANDOM_TABLE', conn, index=False)
pd.read_sql("""select * from RANDOM_TABLE""", conn)
conn.close()
```

`mysql`
```bash
$ pip install mysql-connector-python
```
```python
import mysql.connector
import numpy as np
import pandas as pd

conn = mysql.connector.connect(
    host="127.0.0.1",  # "localhost", "127.0.0.1"
    user="root",
    password="PASSWORD",
    database="example"
)

cursor = conn.cursor()
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

cursor.close()
conn.close()
```
```python

```

