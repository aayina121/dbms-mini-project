# dbms-mini-project
CREATE DATABASE LibraryManagement;
USE LibraryManagement;
-- Library Table
CREATE TABLE Library (
 library_id INT PRIMARY KEY,
 name VARCHAR(100),
 address VARCHAR(255),
 contact_number VARCHAR(20)
);
-- Book Table
CREATE TABLE Book (
 book_id INT PRIMARY KEY,
 title VARCHAR(100),
 author VARCHAR(100),
 genre VARCHAR(50),
 library_id INT,
 FOREIGN KEY (library_id) REFERENCES Library(library_id)
);
-- Member Table
CREATE TABLE Member (
 member_id INT PRIMARY KEY,
 name VARCHAR(100),
 phone VARCHAR(15),
 email VARCHAR(100)
);
-- Staff Table
CREATE TABLE Staff (
 staff_id INT PRIMARY KEY,
 name VARCHAR(100),
 position VARCHAR(50),
 library_id INT,
 FOREIGN KEY (library_id) REFERENCES Library(library_id)
 );
-- Issuance Table
CREATE TABLE Issuance (
 issue_id INT PRIMARY KEY,
 member_id INT,
 book_id INT,
 issue_date DATE,
 return_date DATE,
 FOREIGN KEY (member_id) REFERENCES Member(member_id),
 FOREIGN KEY (book_id) REFERENCES Book(book_id)
);
-- Fine Table
CREATE TABLE Fine (
 fine_id INT PRIMARY KEY,
 issue_id INT,
 amount DECIMAL(10, 2),
 fine_date DATE,
 FOREIGN KEY (issue_id) REFERENCES Issuance(issue_id)
);

Sample Data:
-- Library
INSERT INTO Library VALUES (1, 'Central Library', 'New Delhi',
'9999999999');
-- Books
INSERT INTO Book VALUES (101, 'The Alchemist', 'Paulo Coelho', 'Fiction',
1);
INSERT INTO Book VALUES (102, 'Introduction to SQL', 'John Smith',
'Education', 1);
-- Members
INSERT INTO Member VALUES (1, 'Rahul Verma', '9876543211',
'rahul@gmail.com');
INSERT INTO Member VALUES (2, 'Neha Singh', '9876543212',
'neha@gmail.com');
-- Staff
INSERT INTO Staff VALUES (10, 'Anita Roy', 'Librarian', 1);
INSERT INTO Staff VALUES (11, 'Rajiv Mehta', 'Assistant', 1);
-- Issuance
INSERT INTO Issuance VALUES (5001, 1, 101, '2025-04-01', '2025-04-10');
INSERT INTO Issuance VALUES (5002, 2, 102, '2025-04-02', '2025-04-08');
-- Fine
INSERT INTO Fine VALUES (9001, 5001, 100.00, '2025-04-11');
INSERT INTO Fine VALUES (9002, 5002, 0.00, '2025-04-08');
7. SQL Queries and Outputs
1. Show All Issuances
SELECT * FROM Issuance;
Output:
issue_id member_id book_id issue_date return_date
5001 1 101 2025-04-01 2025-04-10
5002 2 102 2025-04-02 2025-04-08
   2. View Books in Central Library
SELECT Book.book_id, Book.title, Book.author
FROM Book
JOIN Library ON Book.library_id = Library.library_id
WHERE Library.name = 'Central Library';
Output:
book_id title author
101 The Alchemist Paulo Coelho
102 Introduction to SQL John Smith


3. Total Fine by Each Member
SELECT Member.name, SUM(Fine.amount) AS Total_Fine
FROM Fine
JOIN Issuance ON Fine.issue_id = Issuance.issue_id
JOIN Member ON Issuance.member_id = Member.member_id
GROUP BY Member.name;

Output:
name Total_Fine
Rahul Verma 100.00
Neha Singh 0.00

5. List Members with Issue Dates
SELECT Member.name, Issuance.issue_date
FROM Member
JOIN Issuance ON Member.member_id = Issuance.member_id;
Output:
name issue_date
Rahul Verma 2025-04-01
Neha Singh 2025-04-02

6. List All Staff in the Library
   SELECT * FROM Staff;
   
Output:
staff_id name position library_id
10 Anita Roy Librarian 1
11 Rajiv Mehta Assistant 1
