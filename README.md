# SQL-Questions-on-Sakila-Database

Solve  some basic questions on Sakila Database Like Joins nested Queries

use sakila;

show tables;

select  * from actor;
select  * from adress;
select  * from category;
select  * from country;
select  * from city;
select  * from customer;
select  * from film ;
select  * from film_actor;
select  * from film_category;
select  * from film_text;
select  * from inventory;
select  * from langauge;
select  * from payment;
select  * from rental;
select  * from staff;
select  * from store;

-- Q1. Display the first and last name from table actor?

select first_name , last_name from actor;

-- Q2. Display all actor whose last_name is "harris"?

select first_name , last_name from actor where last_name = "harris";

-- Q3. Display all names  together as single name?

select lower(concat(first_name , " " , last_name)) as Full_Name from actor;

-- Q4. Display  ID number first name and lst name from the actor table?

select actor_id , first_name , last_name from  actor where first_name = "BOB";

-- Q5. How many distinct actors last name are there?

select count(distinct(last_name)) as Unique_names from actor;

-- Q6. Display only those last_name which are not reapeating?

select last_name , count(last_name) as count from actor group by last_name having count(*) = 1;

-- Q7. Which last name appear more than once?

select last_name , count(last_name) as count from actor group by last_name having count(*) > 1;

-- Q8. Display the last_name and number of times its occuring in actor?

select last_name , count(last_name) as count from actor group by last_name order by count desc;

-- Q9. Display first name , lst name and address of all staffs?

select s.first_name , s.last_name , a.address from 
staff s left join address a
on
s.address_id = a.address_id;

-- Q10. Which actor has appered in the most films?

select a.actor_id, concat(a.first_name , " " , last_name) as Full_name , count(fa.film_id) as film_count
from 
actor a left join film_actor fa
on 
a.actor_id = fa.actor_id
group by   a.actor_id
order by film_count desc
limit 1;

-- Q11. What is the average length of film in Sakila DB?

select avg(length) as avg_length from film;

-- Q12. What is the average length of film by category?

select c.name,  avg(t1.length) as avg_len from 
(select f.film_id, f.title , f.length , fc.category_id
from 
film f left join film_category fc
on f.film_id = fc.film_id) t1
left join  category c
on t1.category_id = c.category_id
group by name
order by avg_len desc;

-- Q13. which category is longer than overall avg movie length

select c.name as category_name,  avg(t1.length) as avg_len from 
(select f.film_id, f.title , f.length , fc.category_id
from 
film f left join film_category fc
on f.film_id = fc.film_id) t1
left join  category c
on t1.category_id = c.category_id
group by name
having avg_len > (select avg(length) from film);

-- Q14. Which Genre films are the most rented in our inventory?

select * from film;
select * from film_category;
select * from inventory;
select * from rental;

select name as movie_category , count(rental_id) as rental_count from
(select t2.name , r.rental_id from 
(select t1.category_id, t1.name, t1.film_id , i.inventory_id from 
(select c.category_id , c.name , fc.film_id from category c
right join film_category fc
on c.category_id = fc.category_id) t1
inner join inventory i
on t1.film_id = i.film_id) t2 
inner join rental r
on t2.inventory_id = r.inventory_id ) t3
group by name
order by rental_count desc limit 1;

-- Q15.  Use "JOIN" tp display the total amount rung up by each staff member in August of 2005 ?

select * from staff;
select * from payment;


select concat(s.first_name , " " , s.last_name) as staff_name, sum(p.amount) as total_revenue from staff s
left join payment p
on s.staff_id = p.staff_id
where 
month(p.payment_date) = 8
and 
year(p.payment_date) = 2005
group by staff_name ; 


-- Q16. List each  film and the number of actors who are listed for that film. ( use tables film_actor and film)?

select f.title , count(fc.actor_id) as actor_count from film f
left join film_actor fc
on f.film_id = fc.film_id
group by title
order by actor_count desc;













