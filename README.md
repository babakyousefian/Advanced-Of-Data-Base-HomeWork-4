# Advanced-Of-Data-Base-HomeWork-4
Advanced Database Homework 4 Deadline: 1403/09/05 Notes: ➢ Please upload your source codes and reports in Dropbox. ➢ Any submission after the deadline is not considered and you will not get any credit.

---
---
---

# Advanced Database Homework 4 - MongoDB Project

This project is a MongoDB-based implementation for managing a university database named `LorestanUniv`. The project includes the creation of collections, schema validation, CRUD operations, user roles, transactions, and views, all performed entirely in `mongosh`.

---

## **Project Features**
The project involves the following components and functionalities:

### 1. **Database Name**
- **`LorestanUniv`**: The primary database for storing university-related data.

---

### 2. **Collections**
The project uses the following collections with detailed schema validation:

#### **1. `student`**
Fields:
- `STID` (Student ID): 11-digit number.
- `FName`, `LName`, `Father`: Persian characters only, max length 10.
- `Birth`: Date in Persian format (`YYYY-MM-DD`).
- `BornCity`: A valid city in Iran.
- `Address`: String, max length 100.
- `PostalCode`: 10-digit number.
- `CPhone`: Valid Iranian mobile number.
- `HPhone`: Valid Iranian landline number.
- `Department`: Valid departments (e.g., Engineering, Science).
- `Major`: Valid majors (e.g., Computer Science).
- `Married`: Status as either "Single" or "Married".
- `ID`: Valid Iranian national ID.

#### **2. `lecturer`**
Fields:
- `LID` (Lecturer ID): 6-digit number.
- `FName`, `LName`: Persian characters only, max length 10.
- `Birth`: Date in Persian format (`YYYY-MM-DD`).
- `BornCity`, `Address`, `PostalCode`, `CPhone`, `HPhone`, `ID`, `Department`, `Major`: Similar to `student`.

#### **3. `course`**
Fields:
- `CID` (Course ID): 5-digit number.
- `CName`: Persian characters only, max length 25.
- `Department`: Valid departments (e.g., Engineering, Arts).
- `Credit`: Integer between 1 and 4.

#### **4. `CourseRegister`**
Tracks student course registrations:
- Combines `student` and `course` collections using `View`.

#### **5. `PresentedCourses`**
Tracks courses presented by lecturers:
- Combines `lecturer` and `course` collections using `View`.

---

### 3. **Views**
Two views are created for derived data:
1. **`CourseRegister`**: Links `student` and `course` data.
2. **`PresentedCourses`**: Links `lecturer` and `course` data.

---

### 4. **CRUD Operations**
For each collection, the following operations are supported:
- **INSERT**: Add a new record.
- **UPDATE**: Modify an existing record.
- **DELETE**: Remove a record.
- **READ**: Retrieve records.

---

### 5. **User Roles**
Three users are created with specific roles:
1. **Saman**: Full administrative access (`dbOwner`).
2. **Armin**: Read-only access to all collections.
3. **Sara**:
   - Write access to the `student` collection.
   - Read-only access to all collections.

---

### 6. **Transactions**
A multi-step transaction is implemented to ensure data integrity:
1. **Delete a record** from the `course` collection.
2. **Delete related records** from `CourseRegister`.
3. **Delete related records** from `PresentedCourses`.
4. Includes transaction states: `ACTIVE`, `PARTIALLY COMMITTED`, `COMMITTED`, `FAILED`, `SAVEPOINT`, `ROLLBACK`, `BEGIN`, and `ABORTED`.

---

### **How to Use the Project**

#### **1. Setup**
1. Start `mongosh` and connect to MongoDB.
2. Create the `LorestanUniv` database:
   ```javascript
   use LorestanUniv;

2. Create Collections

Define the collections with schema validation as described.
3. Insert Data

Add sample data to the collections:

```bash

db.student.insertOne({
    STID: "12345678901",
    FName: "علی",
    LName: "رضایی",
    Father: "حسین",
    Birth: "1375-03-15",
    BornCity: "تهران",
    Address: "خیابان آزادی",
    PostalCode: "1234567890",
    CPhone: "09123456789",
    HPhone: "02112345678",
    Department: "Engineering",
    Major: "Computer Science",
    Married: "Single",
    ID: "1234567890"
});

```

4. Create Views

Create views for CourseRegister and PresentedCourses using $lookup and $project.
5. Create Users

Create the users with their roles:

```bash

db.createUser({
    user: "Saman",
    pwd: "admin_password",
    roles: [{ role: "dbOwner", db: "LorestanUniv" }]
});

db.createUser({
    user: "Armin",
    pwd: "readonly_password",
    roles: [{ role: "read", db: "LorestanUniv" }]
});

db.createUser({
    user: "Sara",
    pwd: "restricted_password",
    roles: [
        { role: "read", db: "LorestanUniv" },
        { role: "readWrite", db: "LorestanUniv", collection: "student" }
    ]
});

```

6. Transactions

Execute the transaction to delete a course and related records:

```bash

session = db.getMongo().startSession();
db = session.getDatabase("LorestanUniv");

try {
    session.startTransaction();

    // Delete course
    db.course.deleteOne({ CID: "12345" }, { session });

    // Delete related CourseRegister entries
    db.CourseRegister.deleteMany({ CID: "12345" }, { session });

    // Delete related PresentedCourses entries
    db.PresentedCourses.deleteMany({ CID: "12345" }, { session });

    session.commitTransaction();
    print("Transaction committed successfully.");
} catch (error) {
    session.abortTransaction();
    print("Transaction aborted: ", error.message);
} finally {
    session.endSession();
}

```

Project Structure

    - - LorestanUniv: Database name.
    - - Collections: student, lecturer, course, CourseRegister, PresentedCourses.
    - - Views: CourseRegister, PresentedCourses.

Notes

    All operations are performed in mongosh.
    Validation rules ensure data integrity.
    Transactions ensure atomicity for critical operations.

# @Author Babak yousefian

Advanced Database Coursework, Lorestan University


---

### Download the `README.md` File
You can download the `README.md` file here:

```plaintext
Click to Download: [README.md](sandbox:/mnt/data/README.md)
```
---

