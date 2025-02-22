# 📚 Library Management System

This is a **SQL-based Library Management System** that tracks books, members, and borrowing records. It includes **database schema, sample data, and useful queries** for managing a library efficiently.

![Library Management Banner](https://user-images.githubusercontent.com/yourusername/library-banner.png) *(Optional: Add an image here)*

## 📌 Features
- Store and manage books, members, and borrowing history.
- Track which books are borrowed and who borrowed them.
- Search for books by **title, author, or genre**.
- Find overdue books and update book availability after returns.
- Identify the **most borrowed genres**.

---

## 📂 Database Schema

### 📘 Books Table (`Books`)
Stores information about all books in the library.

| Column         | Type             | Description                           |
|---------------|-----------------|---------------------------------------|
| `BookID`      | `INT PRIMARY KEY AUTO_INCREMENT` | Unique identifier for each book. |
| `Title`       | `VARCHAR(255) NOT NULL` | Title of the book. |
| `Author`      | `VARCHAR(255) NOT NULL` | Author of the book. |
| `Genre`       | `VARCHAR(100)`   | Genre of the book (e.g., Fiction, Dystopian). |
| `PublishedYear` | `INT`          | Year the book was published. |
| `Available`   | `BOOLEAN DEFAULT TRUE` | Availability status (TRUE = available). |

---

### 🧑 Members Table (`Members`)
Stores details of library members.

| Column       | Type             | Description |
|-------------|-----------------|-------------|
| `MemberID`  | `INT PRIMARY KEY AUTO_INCREMENT` | Unique identifier for each member. |
| `FirstName` | `VARCHAR(100) NOT NULL` | First name of the member. |
| `LastName`  | `VARCHAR(100) NOT NULL` | Last name of the member. |
| `Email`     | `VARCHAR(255) UNIQUE` | Member's email (must be unique). |
| `JoinDate`  | `DATE DEFAULT CURRENT_DATE` | Date the member joined the library. |

---

### 🔄 Borrowing Records Table (`BorrowingRecords`)
Tracks which books are borrowed by which members.

| Column        | Type             | Description |
|--------------|-----------------|-------------|
| `RecordID`   | `INT PRIMARY KEY AUTO_INCREMENT` | Unique identifier for each borrowing record. |
| `BookID`     | `INT, FOREIGN KEY REFERENCES Books(BookID)` | References `Books.BookID`. |
| `MemberID`   | `INT, FOREIGN KEY REFERENCES Members(MemberID)` | References `Members.MemberID`. |
| `BorrowDate` | `DATE DEFAULT CURRENT_DATE` | Date when the book was borrowed. |
| `ReturnDate` | `DATE (NULLABLE)` | Date when the book was returned (NULL if not returned yet). |

---

## 📝 Sample Data

### 📚 Insert Books
```sql
INSERT INTO Books (Title, Author, Genre, PublishedYear) VALUES
('The Great Gatsby', 'F. Scott Fitzgerald', 'Fiction', 1925),
('To Kill a Mockingbird', 'Harper Lee', 'Fiction', 1960),
('1984', 'George Orwell', 'Dystopian', 1949);
```

### 🏠 Insert Members
```sql
INSERT INTO Members (FirstName, LastName, Email) VALUES
('John', 'Doe', 'john.doe@example.com'),
('Jane', 'Smith', 'jane.smith@example.com');
```

### 🔄 Insert Borrowing Records
```sql
INSERT INTO BorrowingRecords (BookID, MemberID, BorrowDate) VALUES
(1, 1, '2023-10-01'),
(2, 2, '2023-10-05');
```

---

## 🔍 Useful Queries

### 📜 1. List All Books
```sql
SELECT * FROM Books;
```

### ✍️ 2. Find Books by a Specific Author (e.g., George Orwell)
```sql
SELECT * FROM Books WHERE Author = 'George Orwell';
```

### 📆 3. List Members Who Joined in 2023
```sql
SELECT * FROM Members WHERE YEAR(JoinDate) = 2023;
```

### 📖 4. Find Currently Borrowed Books (Not Yet Returned)
```sql
SELECT Books.Title, Members.FirstName, Members.LastName, BorrowingRecords.BorrowDate
FROM BorrowingRecords
JOIN Books ON BorrowingRecords.BookID = Books.BookID
JOIN Members ON BorrowingRecords.MemberID = Members.MemberID
WHERE BorrowingRecords.ReturnDate IS NULL;
```

### ✅ 5. Update a Book’s Availability After It’s Returned
```sql
UPDATE Books SET Available = TRUE WHERE BookID = 1;
UPDATE BorrowingRecords SET ReturnDate = CURRENT_DATE WHERE BookID = 1 AND MemberID = 1;
```

### 📊 6. Count the Number of Books Borrowed by Each Member
```sql
SELECT Members.FirstName, Members.LastName, COUNT(BorrowingRecords.RecordID) AS BooksBorrowed
FROM Members
LEFT JOIN BorrowingRecords ON Members.MemberID = BorrowingRecords.MemberID
GROUP BY Members.MemberID;
```

### 🔎 7. Search for Books by Title, Author, or Genre
```sql
SELECT * FROM Books
WHERE Title LIKE '%Gatsby%' OR Author LIKE '%Fitzgerald%' OR Genre LIKE '%Fiction%';
```

### ⏳ 8. Find Overdue Books (Borrowed More Than 30 Days Ago)
```sql
SELECT Books.Title, Members.FirstName, Members.LastName, BorrowingRecords.BorrowDate
FROM BorrowingRecords
JOIN Books ON BorrowingRecords.BookID = Books.BookID
JOIN Members ON BorrowingRecords.MemberID = Members.MemberID
WHERE BorrowingRecords.ReturnDate IS NULL
AND DATEDIFF(CURRENT_DATE, BorrowingRecords.BorrowDate) > 30;
```

### 📈 9. Show Most Popular Genres (Based on Borrowing Records)
```sql
SELECT Books.Genre, COUNT(BorrowingRecords.RecordID) AS BorrowCount
FROM BorrowingRecords
JOIN Books ON BorrowingRecords.BookID = Books.BookID
GROUP BY Books.Genre
ORDER BY BorrowCount DESC;
```

---

## 🚀 How to Use
1. Clone this repository:
   ```bash
   git clone https://github.com/yourusername/library-management.git
   cd library-management
   ```
2. Import the SQL file into **MySQL/PostgreSQL/SQLite**.
3. Run the SQL queries to interact with the library system.
4. Extend functionality by adding **new tables, constraints, or views**.

---

## 💡 Future Enhancements
✅ Add **fines for overdue books**  
✅ Implement **book reservations**  
✅ Add **a web or CLI interface** for easy use  

---

## 🏆 Contributors
- **John Atkins**  
- Open-source contributions are welcome! Feel free to submit a **pull request**.  

---

## 📜 License
This project is open-source and available under the **MIT License**.

---

### ⭐ Show Some Support!
If you found this useful, please **star** 🌟 this repository and **fork** 🍴 it to contribute!  
