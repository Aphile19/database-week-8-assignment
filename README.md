Library Management System
A comprehensive database system for managing library operations including book tracking, member management, loans, reservations, and fines.

Database Schema Overview
This Library Management System is built with MySQL and includes the following tables:

members - Library members information

authors - Book authors details

publishers - Book publishers information

books - Library inventory with status tracking

book_authors - Junction table for many-to-many book-author relationship

loans - Book borrowing records

reservations - Book reservation system

fines - Fine management for overdue books

Relationships
One-to-Many:

Publishers to Books

Members to Loans

Members to Reservations

Books to Loans

Books to Reservations

Loans to Fines

Many-to-Many:

Books to Authors (via book_authors junction table)

Setup Instructions
Prerequisites
MySQL Server (8.0 or higher)

MySQL Command Line Client or MySQL Workbench

Installation
Clone or download the SQL file from this repository

Run the SQL script in your MySQL environment:

bash
mysql -u your_username -p < library_management.sql
Alternatively, you can copy and paste the SQL contents into your MySQL client

Verification
After running the script, verify the database was created correctly:

sql
SHOW DATABASES;
USE library_db;
SHOW TABLES;
Sample Queries
Get all available books
sql
SELECT book_id, title, category FROM books WHERE status = 'Available';
Find books by a specific author
sql
SELECT b.title, b.category, CONCAT(a.first_name, ' ', a.last_name) AS author
FROM books b
JOIN book_authors ba ON b.book_id = ba.book_id
JOIN authors a ON ba.author_id = a.author_id
WHERE a.last_name = 'King';
Check current loans with member information
sql
SELECT 
    l.loan_id,
    b.title,
    CONCAT(m.first_name, ' ', m.last_name) AS member,
    l.loan_date,
    l.due_date
FROM loans l
JOIN books b ON l.book_id = b.book_id
JOIN members m ON l.member_id = m.member_id
WHERE l.status = 'Active';
Calculate total fines by member
sql
SELECT 
    m.member_id,
    CONCAT(m.first_name, ' ', m.last_name) AS member,
    SUM(f.amount) AS total_fines
FROM fines f
JOIN members m ON f.member_id = m.member_id
WHERE f.payment_status = 'Unpaid'
GROUP BY m.member_id;
API Documentation (For Future Implementation)
The database is designed to support a CRUD application with the following potential endpoints:

Members
GET /members - Retrieve all members

POST /members - Create a new member

GET /members/:id - Get specific member details

PUT /members/:id - Update member information

DELETE /members/:id - Deactivate a member

Books
GET /books - Retrieve all books with filtering options

POST /books - Add a new book to inventory

GET /books/:id - Get specific book details

PUT /books/:id - Update book information

DELETE /books/:id - Remove a book from inventory

Loans
GET /loans - View all current loans

POST /loans - Create a new loan

PUT /loans/:id - Return a book (update loan status)

Reservations
GET /reservations - View all active reservations

POST /reservations - Create a new reservation

DELETE /reservations/:id - Cancel a reservation

Constraints and Validation
The database implements several constraints to ensure data integrity:

Primary Keys - All tables have auto-increment primary keys

Foreign Keys - All relationships are enforced with foreign key constraints

Unique Constraints - Email addresses and ISBNs must be unique

NOT NULL - Essential fields are required

ENUM Constraints - Status fields have predefined values

CASCADE Operations - Related records are automatically handled on deletion

Future Enhancements
Add user authentication system

Implement book rating and review system

Add advanced search functionality

Implement email notifications for due dates

Add reporting and analytics features
