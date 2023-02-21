# students_registation
crud Application about student registation

Exple Crud not this app these is just exmple(kotlin)

class MyDatabaseHelper(context: Context) : SQLiteOpenHelper(context, DATABASE_NAME, null, DATABASE_VERSION) {

    companion object {
        private const val DATABASE_VERSION = 1
        private const val DATABASE_NAME = "my_database.db"
    }

    override fun onCreate(db: SQLiteDatabase?) {
        db?.execSQL("CREATE TABLE my_table (id INTEGER PRIMARY KEY AUTOINCREMENT, name TEXT, age INTEGER)")
    }

    override fun onUpgrade(db: SQLiteDatabase?, oldVersion: Int, newVersion: Int) {
        db?.execSQL("DROP TABLE IF EXISTS my_table")
        onCreate(db)
    }

    fun addRecord(name: String, age: Int) {
        val values = ContentValues().apply {
            put("name", name)
            put("age", age)
        }
        writableDatabase.insert("my_table", null, values)
    }

    fun getRecord(id: Int): Record? {
        val cursor = readableDatabase.rawQuery("SELECT * FROM my_table WHERE id = ?", arrayOf(id.toString()))
        return if (cursor.moveToFirst()) {
            Record(
                id = cursor.getInt(cursor.getColumnIndex("id")),
                name = cursor.getString(cursor.getColumnIndex("name")),
                age = cursor.getInt(cursor.getColumnIndex("age"))
            )
        } else {
            null
        }
    }

    fun getAllRecords(): List<Record> {
        val records = mutableListOf<Record>()
        val cursor = readableDatabase.rawQuery("SELECT * FROM my_table", null)
        while (cursor.moveToNext()) {
            records.add(
                Record(
                    id = cursor.getInt(cursor.getColumnIndex("id")),
                    name = cursor.getString(cursor.getColumnIndex("name")),
                    age = cursor.getInt(cursor.getColumnIndex("age"))
                )
            )
        }
        return records
    }

    fun updateRecord(record: Record) {
        val values = ContentValues().apply {
            put("name", record.name)
            put("age", record.age)
        }
        writableDatabase.update("my_table", values, "id = ?", arrayOf(record.id.toString()))
    }

    fun deleteRecord(id: Int) {
        writableDatabase.delete("my_table", "id = ?", arrayOf(id.toString()))
    }
}
In the above code, the MyDatabaseHelper class extends the SQLiteOpenHelper class and provides methods to create, read, update, 
and delete records in the "my_table" table.

data class Record(val id: Int, val name: String, val age: Int)

Now you can use the MyDatabaseHelper class to manage records in your app. For example, you can create a new record like this:


val dbHelper = MyDatabaseHelper(context)
dbHelper.addRecord("John Doe", 30)

To retrieve a record by its ID:

val record = dbHelper.getRecord(1)

To retrieve all records:
val records = dbHelper.getAllRecords()

To update a record:
val record = dbHelper.getRecord(1)
record?.let {
    it.name = "Jane Doe"
    it.age = 35
    dbHelper.updateRecord(it)
}

To delete a record:
dbHelper.deleteRecord(1)

Note that in a real-world app, you would typically use these methods in conjunction with a user interface to allow the user to create, read, update, and delete records. 
Additionally, 
you should handle errors and exceptions appropriately, and validate data before it is added to the database
