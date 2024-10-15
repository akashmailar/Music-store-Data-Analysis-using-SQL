# Music-store-Data-Analysis-using-SQL

## Project overview

**Project Title**: Music-store-Data-Analysis

This project is to analyze the music playlist database. examine the dataset with SQL and helps the store understand it's business growth by answering simple questions.

## Objectives

1. **Set up the Library Management System Database**: Create and populate the database with tables for album, artist, customer, employee, genre, invoice, and more.
2. **CRUD Operations**: Perform Create, Read, Update, and Delete operations on the data.
3. **CTAS (Create Table As Select)**: Utilize CTAS to create new tables based on query results.
4. **Advanced SQL Queries**: Develop complex queries to analyze and retrieve specific data.

### 1. Database Setup

- **Database Creation**: Created a database named `music_store_db`.
- **Table Creation**: Created tables for album, artist, customer, employee, genre, invoice, and more. Each table includes relevant columns and relationships.

### 2. CRUD Operations

- **Create**: Inserted sample records into the table.
- **Read**: Retrieved and displayed data from various tables.
- **Update**: Updated records in the table.
- **Delete**: Removed records from the table as needed.

**QUESTION SET 1 :- EASY**

Q1: Who is the senior most employee based on job title?

Q2: Which countries have the most Invoices? 

Q3: What are top 3 values of total invoice? 

Q4: Which city has the best customers? We would like to throw a promotional Music Festival in the city we made the most money. Write a query that returns one city that has the highest sum of invoice totals. Return both the city name & sum of all invoice totals 

Q5: Who is the best customer? The customer who has spent the most money will be declared the best customer. Write a query that returns the person who has spent the most money.


**QUESTION SET 2 :- MODERATE**

Q1: Write query to return the email, first name, last name, & Genre of all Rock Music listeners. Return your list ordered alphabetically by email starting with A. 

Q2: Let's invite the artists who have written the most rock music in our dataset. Write a query that returns the Artist name and total track count of the top 10 rock bands. 

Q3: Return all the track names that have a song length longer than the average song length. Return the Name and Milliseconds for each track. Order by the song length with the longest songs listed first. 


**QUESTION SET 3 :- ADVANCE**

Q1: Find how much amount spent by each customer on artists? Write a query to return customer name, artist name and total spent.

Q2: We want to find out the most popular music Genre for each country. We determine the most popular genre as the genre with the highest amount of purchases. Write a query that returns each country along with the top Genre. For countries where the maximum number of purchases is shared return all Genres.

Q3: Write a query that determines the customer that has spent the most on music for each country. Write a query that returns the country along with the top customer and how much they spent. For countries where the top amount spent is shared, provide all customers who spent this amount. 


## Conclusion

This project demonstrates the application of SQL skills in creating and managing a music-store-data. It includes database setup, data manipulation, and advanced querying, providing a solid foundation for data management and analysis.


Thank you !
