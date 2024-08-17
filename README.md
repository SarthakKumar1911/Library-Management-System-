# Library-Management-System-

## Project Overview

The Library Management System (LMS) is a robust and cost-effective solution designed to manage the key operations of a library. The system covers essential functions such as tracking book availability, managing borrower details, handling fines, and overseeing membership information. This project is implemented using SQL on Microsoft SQL Server, featuring a well-defined database schema with multiple entities and relationships, such as books, authors, members, and library staff.

### Key Functionalities:
- **For Students:**
  1. Search for books.
  2. Issue, renew, and return books.
  3. Pay fines.

- **For Library Staff:**
  1. Add, delete, and approve books.
  2. View and manage member fines.
  3. Update book details.
  4. Perform all functionalities available to students.

### Requirements:
- Each book has a unique ID number.
- Members can search for books by title, author, category, and publication details.
- Books can have multiple authors, and the library can own multiple copies of each book.
- Members can borrow books, with the system tracking borrow dates and due dates.
- Library staff can monitor who has borrowed a particular book and check membership details.
- Members can reserve books if all copies are currently borrowed.
- Fines are imposed on members for late returns, and members can pay their fines through the system.

This project demonstrates a comprehensive use of SQL to manage a library's data efficiently, providing a practical tool for handling day-to-day library operations.

## Data Source

The data for this project is managed within a Microsoft SQL Server database. The database schema includes multiple tables representing the key entities required for library management, such as books, members, authors, publishers, and library staff. The `LMS_With Analysis.sql` script contains all the necessary SQL commands for creating the database, populating it with sample data, and running queries for various operations.

### Key Components of the SQL Script:
- **Database Creation**: The script starts by creating a new database named `lib_mngmt_system`.
- **Table Definitions**: The script includes the creation of all necessary tables with primary keys, foreign keys, and other constraints to enforce data integrity.
- **Data Insertion**: Sample data is inserted into the tables to simulate a functioning library system.

  
- **Query Execution**: The script provides various SQL queries to handle operations like book issues, returns, fine calculations, and member management.

The SQL script effectively demonstrates the practical use of SQL in managing a comprehensive library system, supporting all key operations from data entry to complex queries.

## Tools & Technologies

This project was built and analyzed using the following tools:

- **Microsoft SQL Server**: Used for database creation, data management, and executing SQL queries to handle library operations.
- **Draw.io**: Utilized for creating the Entity-Relationship (ER) Diagram, which visually represents the database schema and relationships between entities.
- **Markdown**: Used for creating project documentation, including this `README.md` file.

These tools together provide a comprehensive environment for designing, implementing, and documenting the Library Management System.

## Repository Structure

The repository is organized as follows:

```
Library-Management-System/
├── data/
│   └── LMS_With Analysis.sql             # SQL script containing database creation, data insertion, and queries
├── docs/
│   └── Library Management System(LMS).pdf # Project documentation and details
├── images/
│   ├── ER Diagram_LMS.drawio             # Editable ER Diagram file
│   └── ER-Diagram LMS.jpg                # Image of the ER Diagram
└── README.md                             # Project overview and documentation
```

### Explanation:
- **data/**: Contains the SQL script for creating and managing the database.
- **docs/**: Includes detailed project documentation in PDF format.
- **images/**: Holds the Entity-Relationship Diagram files in both editable and image formats.
- **README.md**: Provides an overview and instructions for the project.

This structure ensures that all relevant files are organized and accessible, making it easy for users to navigate and understand the project.


## Key Features

The Library Management System (LMS) includes several key features that make it a comprehensive and efficient tool for managing library operations:

1. **Comprehensive Book Management**:
   - Track detailed information about each book, including title, author, category, publisher, and availability.
   - Manage multiple copies of books and their respective locations within the library.

2. **Member Management**:
   - Maintain complete records of library members, including personal information and membership status.
   - Support for different membership types (e.g., student, professional, staff) with distinct account statuses (active, inactive).

3. **Book Issue and Return**:
   - Facilitate the issuing and returning of books, with automated tracking of due dates and fines for overdue items.
   - Enable the reservation of books that are currently checked out.

4. **Fine Management**:
   - Automatically calculate and track fines for overdue books.
   - Record fine payments and maintain a history of fines for each member.

5. **Reporting and Queries**:
   - Execute predefined SQL queries to generate reports on book availability, member activity, fine details, and more.
   - Perform advanced searches, such as finding all inactive members, books in specific categories, or books issued by certain members.

These features collectively provide a powerful tool for managing a library’s operations efficiently and effectively.

## SQL Queries

Below is a comprehensive list of SQL queries that demonstrate the core functionalities of the Library Management System (LMS). These queries are essential for managing books, members, fines, and other operations within the system.

### 1. Display All Members with Membership Details
This query retrieves all members along with their membership status and details.

```sql
SELECT m.member_id, m.first_name, m.last_name, m.city, ms.account_type, 
       ms.account_status, ms.membership_start_date, ms.membership_end_date
FROM tbl_member m
JOIN tbl_member_status ms ON m.active_status_id = ms.active_status_id;
```

### 2. Find Inactive Members
This query identifies all members whose accounts are currently inactive.

```sql
SELECT m.member_id, m.first_name, m.last_name, m.city, ms.account_type, 
       ms.account_status, ms.membership_start_date, ms.membership_end_date
FROM tbl_member m
JOIN tbl_member_status ms ON m.active_status_id = ms.active_status_id
WHERE ms.account_status = 'inactive';
```

### 3. Display Members with Fine Due
This query lists all members who have outstanding fines.

```sql
SELECT m.member_id, m.first_name, m.last_name, m.city, f.fine_date, f.fine_total, 
       ms.account_type, ms.account_status, ms.membership_start_date, ms.membership_end_date
FROM tbl_member m
JOIN tbl_fine_due f ON m.member_id = f.member_id
JOIN tbl_member_status ms ON m.active_status_id = ms.active_status_id;
```

### 4. Count Total Number of Books in the Library
This query calculates the total number of books available in the library.

```sql
SELECT SUM(copies_total) AS total_books FROM tbl_book;
```

### 5. Display All Books Available for Borrowing
This query shows the number of books that are currently available for borrowing.

```sql
SELECT SUM(copies_available) AS books_available_for_borrowing FROM tbl_book;
```

### 6. Display Book Count by Category
This query provides the total number of books for each category in the library.

```sql
SELECT c.category_name, SUM(b.copies_total) AS total_books
FROM tbl_category c
JOIN tbl_book b ON b.category_id = c.category_id
GROUP BY c.category_name
ORDER BY total_books DESC;
```

### 7. Issue a Book to a Member
This query issues a book to a member and updates the number of available copies.

```sql
INSERT INTO tbl_book_issue (book_id, member_id, issue_date, return_date, issue_status, issued_by_id)
VALUES (10, 4, '2022-11-20', '2022-12-05', 'underdue', 1);

UPDATE tbl_book 
SET copies_available = copies_available - 1 
WHERE book_id = 10;
```

### 8. Return a Book and Update Availability
This query returns a book to the library and updates the availability status.

```sql
UPDATE tbl_book_issue 
SET issue_status = 'returned' 
WHERE book_id = 10 AND member_id = 4;

UPDATE tbl_book 
SET copies_available = copies_available + 1 
WHERE book_id = 10;
```

### 9. Search for a Specific Book
This query searches for a book by title and checks if it is available for borrowing.

```sql
SELECT * FROM tbl_book 
WHERE book_title = 'Three Thousand Stitches';
```

### 10. Display Fine Details for a Specific Member
This query retrieves the fine details for a specific member by name.

```sql
SELECT m.member_id, m.first_name, m.last_name, m.city, f.fine_date, f.fine_total, 
       ms.account_type, ms.account_status, ms.membership_start_date, ms.membership_end_date
FROM tbl_member m
JOIN tbl_fine_due f ON m.member_id = f.member_id
JOIN tbl_member_status ms ON m.active_status_id = ms.active_status_id
WHERE m.first_name = 'A' AND m.last_name = 'Kumar';
```

These queries demonstrate a wide range of functionalities, from managing members and books to handling fines and issuing books, showcasing the depth of the Library Management System.


## Visualizations

### Entity-Relationship Diagram (ERD)

The Entity-Relationship Diagram (ERD) provides a visual representation of the database schema, illustrating the relationships between different entities within the Library Management System. This diagram is crucial for understanding how data is structured and connected across various tables.

#### Key Entities in the ERD:
- **Books**: Contains all information related to the books in the library, such as title, author, category, and availability.
- **Members**: Stores detailed records of library members, including their personal information and membership status.
- **Authors**: Manages information about the authors of the books.
- **Book Issue**: Tracks the borrowing and returning of books, including issue dates, return dates, and fine details.
- **Fine**: Records and manages fines imposed on members for overdue books.

### ERD Visualization
Below is the Entity-Relationship Diagram for the Library Management System:

![ER Diagram](images/ER-Diagram%20LMS.jpg)

The ERD highlights the key relationships, such as:
- Books can have multiple authors.
- Members can borrow multiple books.
- The system tracks the status of issued books and imposes fines for overdue returns.

This visualization helps in understanding the overall structure and data flow within the Library Management System, providing a clear overview of how different entities interact with each other.

## How to Use This Repository

This repository contains all the necessary files and documentation to understand, replicate, and extend the Library Management System project.

### 1. Setting Up the Database
- Download or clone the repository to your local machine.
- Navigate to the `data/` folder and locate the `LMS_With Analysis.sql` file.
- Open the SQL script file in Microsoft SQL Server.
- Execute the script to create the database, tables, and relationships.
- Run the data insertion queries to populate the database with sample data.

### 2. Exploring the Documentation
- The `docs/` folder contains a PDF file (`Library Management System(LMS).pdf`) that provides detailed project documentation, including descriptions of the database schema, functionality, and sample queries.

### 3. Viewing the ER Diagram
- The `images/` folder contains the ER diagram of the database schema:
  - `ER Diagram_LMS.drawio`: Editable Draw.io file for the ER diagram.
  - `ER-Diagram LMS.jpg`: Image file of the ER diagram.
- Open the `ER-Diagram LMS.jpg` file to view the relationships between different entities in the database.

## Conclusion

The Library Management System (LMS) provides a comprehensive solution for managing the essential functions of a library, including book inventory, member management, book issuance, fine handling, and more. By leveraging a well-structured database schema and SQL queries, this system ensures efficient and effective management of library operations.

The project demonstrates the practical application of SQL in a real-world scenario, showing how data can be organized, queried, and maintained for optimal performance. The ER diagram provides a clear visualization of the relationships between different entities, making it easier to understand the data flow within the system.

This repository serves as a robust foundation for anyone looking to implement or extend a library management system using SQL. With its detailed documentation, organized structure, and practical functionality, the LMS project is a valuable tool for libraries of all sizes.

### Future Enhancements:
- **Integration with a front-end interface**: To make the system more user-friendly, a web-based or desktop interface can be developed to interact with the database.
- **Automation of reports**: Generate automatic reports for overdue books, fine summaries, and member activity.
- **Enhanced security features**: Implement user authentication and access control to secure sensitive information.

This project can be further extended and adapted to fit the specific needs of any library, making it a highly adaptable and scalable solution.


### 4. Extending the Project
- You can modify the existing SQL queries or add new ones to extend the functionality of the system.
- Update the ER diagram using the editable Draw.io file if changes are made to the database schema.

By following these steps, you can set up, explore, and extend the Library Management System on your local environment.

