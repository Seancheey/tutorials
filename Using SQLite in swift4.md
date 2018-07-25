Using SQLite in swift4
=======
1: Importing SQLite3
---
In swift 4 xcode 9, the only thing you need to do is this line. Simple.
```swift
import SQLite3
```

2: Creating db pointer
---
First we need to create a db pointer for future use, and use sqlite3_open function to assign value to the database pointer. Since the original code is written in Objective-C, we have to declare the db as a OpaquePointer.
```swift
var db: OpaquePointer?
let dbFile = try! FileManager.default.url(for: .documentDirectory, in: .userDomainMask, appropriateFor: nil, create: false).appendingPathComponent([your filename])
assert(sqlite3_open(dbFile.path, &db) == SQLITE_OK)
```
Here I used an assert to ensure that open function returns a SQLITE_OK, 

2: Executing simple SQL statements
---
Here we will make use of the db pointer we declared earlier to execute a statement.
```swift
assert(sqlite3_exec(db!, "CREATE TABLE IF NOT EXISTS foo(id INTEGER primary key)", nil, nil, nil) == SQLITE_OK)
```
The prototype of the function: [reference](https://www.sqlite.org/c3ref/exec.html)
```
int sqlite3_exec(
  sqlite3*,                                  /* An open database */
  const char *sql,                           /* SQL to be evaluated */
  int (*callback)(void*,int,char**,char**),  /* Callback function */
  void *,                                    /* 1st argument to callback */
  char **errmsg                              /* Error msg written here */
);
```

3: Executing SQL statements with arguments
---
An example of insertion statement. Note that first argument's position is 1, second is 2, etc.
```swift
var statement: OpaquePointer?
assert(sqlite3_prepare_v2(db!, "INSERT INTO foo(id, text, time) values(?,?,?)", -1, &statement, nil) == SQLITE_OK)
assert(sqlite3_bind_int(statement, 1, Int32(123123) == SQLITE_OK)
assert(sqlite3_bind_text(statement, 2, "noodles", -1, SQLITE_TRANSIENT) == SQLITE_OK)
assert(sqlite3_bind_double(statement, 3, date.timeIntervalSince1970) == SQLITE_OK)
// run the statement
assert(sqlite3_step(statement) == SQLITE_DONE)
```
Two things to notice:
1. sqlite3_bind_text used a variable called SQLITE_TRANSIENT, which is not defined in SQLite3 library. This is because the variable was originally contained in .h file in Obj-C, and is not included when importing. You can either replace it with nil or define them as:
```swift
let SQLITE_TRANSIENT = unsafeBitCast(-1, to: sqlite3_destructor_type.self)
```
2.  the prepare function is named as sqlite3_prepare_v2. You should try avoid using sqlite3_prepare function because that is a legacy.

The prototype of the function: [reference](https://www.sqlite.org/c3ref/prepare.html)
```
int sqlite3_prepare_v2(
  sqlite3 *db,            /* Database handle */
  const char *zSql,       /* SQL statement, UTF-8 encoded */
  int nByte,              /* Maximum length of zSql in bytes. */
  sqlite3_stmt **ppStmt,  /* OUT: Statement handle */
  const char **pzTail     /* OUT: Pointer to unused portion of zSql */
);
```

4: Executing SQL statements with return values
---
Here is an example of doing select statement in sqlite3. Remember that the value at first column is at 0 index, and second is at 1, etc.
```swift
var statement: OpaquePointer?
assert(sqlite3_prepare_v2(db!, "SELECT * FROM bar(id,text,time)", -1, &statement, nil) == SQLITE_OK)
while sqlite3_step(statement) == SQLITE_ROW{
	let id = Int(sqlite3_column_int(statement, 0))
	let text = String(cString: sqlite3_column_text(statement, 1))
	let time = Date(timeIntervalSince1970: sqlite3_column_double(statement, 2))
	print("\(id)\t \(text)\t \(time)")
}
```

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTg3MDk0NTIxNF19
-->