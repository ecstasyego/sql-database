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

## 

