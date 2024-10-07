# Music-store-Data-Analysis-using-SQL
This project is to analyze the music playlist database. examine the dataset with SQL and helps the store understand it's business growth by answering simple questions.

-- QUESTION SET 1 :- EASY

-- Q1: Who is the senior most employee based on job title?

select *
from employee
order by levels desc
limit 1;


-- Q2: Which countries have the most Invoices? 

select billing_country, count(invoice_id) as total_invoices
from invoice
group by billing_country
order by total_invoices desc
limit 1;


-- Q3: What are top 3 values of total invoice? 

select *
from invoice
order by total desc
limit 3;


-- Q4: Which city has the best customers? We would like to throw a promotional Music Festival 
--     in the city we made the most money. Write a query that returns one city that has the 
--     highest sum of invoice totals. Return both the city name & sum of all invoice totals 

select c.city as city, round(sum(i.total)) as total
from customer as c
join invoice as i
on c.customer_id = i.customer_id
group by c.city
order by total desc
limit 1;


-- Q5: Who is the best customer? The customer who has spent the most money will be declared 
--     the best customer. Write a query that returns the person who has spent the most money.

select c.customer_id, c.first_name, c.last_name, round(sum(i.total)) as total
from customer as c
join invoice as i 
on c.customer_id = i.customer_id
group by c.customer_id
order by total desc
limit 1;


-- QUESTION SET 2 :- MODERATE

-- Q1: Write query to return the email, first name, last name, & Genre of all Rock Music listeners. 
--     Return your list ordered alphabetically by email starting with A. 

select distinct c.email, c.first_name, c.last_name
from customer as c
join invoice as i on c.customer_id = i.customer_id
join invoice_line as il on il.invoice_id = i.invoice_id
where il.track_id in
(select track_id
from genre as g
join track as t
on g.genre_id = t.genre_id
where g.name like 'Rock')
order by c.email;

-- OR --

select distinct c.email, c.first_name, c.last_name, g.name as song_genre
from customer as c
join invoice as i on i.customer_id = c.customer_id
join invoice_line as il on il.invoice_id = i.invoice_id
join track as t on t.track_id = il.track_id
join genre as g on g.genre_id = t.genre_id
where g.name like 'Rock'
order by email;


-- Q2: Let's invite the artists who have written the most rock music in our dataset. 
--     Write a query that returns the Artist name and total track count of the top 10 rock bands. 

select a.artist_id, a.name as artist_name, count(t.track_id) as track_count
from artist as a
join album as al on a.artist_id = al.artist_id
join track as t on t.album_id = al.album_id
where t.track_id in
(select t.track_id
from genre as g
join track as t on g.genre_id = t.genre_id
where g.name like 'Rock')
group by a.name, a.artist_id
order by track_count desc
limit 10;

-- OR --

select a.artist_id, a.name, count(a.artist_id) as no_of_songs
from artist as a
join album as al on a.artist_id = al.artist_id
join track as t on al.album_id = t.album_id
join genre as g on g.genre_id = t.genre_id
where g.name like 'Rock'
group by a.artist_id
order by no_of_songs desc
limit 10;


-- Q3: Return all the track names that have a song length longer than the average song length. 
--     Return the Name and Milliseconds for each track. Order by the song length with the 
--     longest songs listed first. 

select name, milliseconds
from track
where milliseconds > (select avg(milliseconds) as avg_length
from track)
order by milliseconds desc;


-- QUESTION SET 3 :- ADVANCE

-- Q1: Find how much amount spent by each customer on artists? Write a query to return 
--     customer name, artist name and total spent.

with best_selling_artist as (
select a.artist_id, a.name as artist_name, sum(il.unit_price * il.quantity) as total_sales
from invoice_line as il
join track as t on t.track_id = il.track_id
join album as al on al.album_id = t.album_id
join artist as a on a.artist_id = al.artist_id
group by 1
order by 3 desc
limit 1
)

select c.customer_id, c.first_name, c.last_name, bsa.artist_name, 
       sum(il.unit_price*il.quantity) as amount_spend
from customer as c
join invoice as i on c.customer_id = i.customer_id
join invoice_line as il on il.invoice_id = i.invoice_id
join track as t on t.track_id = il.track_id
join album as al on al.album_id = t.album_id
join best_selling_artist as bsa on bsa.artist_id = al.artist_id
group by 1, 2, 3, 4
order by 5 desc;


-- Q2: We want to find out the most popular music Genre for each country. We determine 
--     the most popular genre as the genre with the highest amount of purchases. 
--     Write a query that returns each country along with the top Genre. For countries where 
--     the maximum number of purchases is shared return all Genres.

with popular_genre as (
select c.country, g.genre_id, g.name as genre_name, count(il.quantity) as total_quantity,
       rank() over(partition by c.country order by count(il.quantity) desc) as ranking
from customer as c
join invoice as i on c.customer_id = i.customer_id
join invoice_line as il on il.invoice_id = i.invoice_id
join track as t on t.track_id = il.track_id
join genre as g on g.genre_id = t.genre_id
group by 1, 2, 3
order by 1 asc, 4 desc
)

select country, genre_id, genre_name, total_quantity
from popular_genre 
where ranking <= 1;


-- Q3: Write a query that determines the customer that has spent the most on music for each country. 
--     Write a query that returns the country along with the top customer and how much they spent. 
--     For countries where the top amount spent is shared, provide all customers who spent this amount. 

with top_most_spend as (
select c.customer_id, c.first_name, c.last_name, i.billing_country, sum(i.total) as total,
       rank() over(partition by i.billing_country order by sum(i.total) desc) as ranking
from customer as c
join invoice as i on c.customer_id = i.customer_id
group by 1, 2, 3, 4
order by  4 asc, 5 desc
)

select customer_id, first_name, last_name, billing_country, total
from top_most_spend
where ranking <= 1;

-- OR --

with recursive
	customer_with_country as (select c.customer_id, first_name, last_name, billing_country, 
                                 sum(total) as total
		from customer as c
		join invoice as i on c.customer_id = i.customer_id
		group by 1, 2, 3, 4
		order by 1, 5 desc
		),

	country_max_spending as(
		select billing_country, max(total) as max_spending
		from customer_with_country
		group by billing_country
)

select cc.billing_country, cc.first_name, cc.last_name, cc.total
from customer_with_country as cc
join country_max_spending as ms on cc.billing_country = ms.billing_country
where cc.total = ms.max_spending
order by 1

