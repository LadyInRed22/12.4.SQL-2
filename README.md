# Домашнее задание к занятию 12.4. «SQL. Часть 2» - `Виноградова Татьяна`
---

### Задание 1

Одним запросом получите информацию о магазине, в котором обслуживается более 300 покупателей, и выведите в результат следующую информацию:

•	фамилия и имя сотрудника из этого магазина;

•	город нахождения магазина;

•	количество пользователей, закреплённых в этом магазине.

``` SQL
select concat(s.first_name, " ", s.last_name) as 'Staff Name', c2.city as City, count(*) as Customers  
  from staff s 
  join store s2 on s2.manager_staff_id  = s.staff_id 
  join customer c on c.store_id = s2.store_id
  join address a on a.address_id = s2.address_id 
  join city c2 on c2.city_id = a.city_id 
  group by 1, 2
  having  Customers > 300 ;
```

### Задание 2

Получите количество фильмов, продолжительность которых больше средней продолжительности всех фильмов.
``` SQL
select count(*) 
  from film f 
  where (select avg(f2.`length`) from film f2) < f.`length` ;
```

### Задание 3

Получите информацию, за какой месяц была получена наибольшая сумма платежей, и добавьте информацию по количеству аренд за этот месяц.
``` SQL
select month(r.rental_date) as 'Месяц', count(*) as 'Количество аренд'
  from rental r 
  where (select month(p.payment_date) as month from payment p group by month order by count(*) desc limit 1) = month(r.rental_date)
  group by 1 ;
``` 

### Задание 4*

Посчитайте количество продаж, выполненных каждым продавцом. Добавьте вычисляемую колонку «Премия». 
Если количество продаж превышает 8000, то значение в колонке будет «Да», иначе должно быть значение «Нет».
``` SQL
select concat(s.first_name, ' ', s.last_name) as StaffName, rentals,
  case 
    when rentals > 8000 then 'Да'
    when rentals < 8000 then 'Нет'
    else 'Нет'
  end as 'Премия'	
  from staff s 
  join 
  	(select staff_id, count(r.rental_id) as rentals from rental r 
  		group by staff_id) q on s.staff_id = q.staff_id ;
``` 

### Задание 5*

Найдите фильмы, которые ни разу не брали в аренду.
``` SQL
select f.title as 'Фильмы, не бравшиеся в аренду'
  from film f 
  left join inventory i on i.film_id = f.film_id 
  where i.inventory_id is null ;
 
``` 
---
