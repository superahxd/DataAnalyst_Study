~~문제1번)~~ 영화 배우가,  영화 180분 이상의 길이 의 영화에 출연하거나, 영화의 rating 이 R 인 등급에 해당하는 영화에 출연한  영화 배우에 대해서,  영화 배우 ID 와 (180분이상 / R등급영화)에 대한 Flag 컬럼을 알려주세요.

- film_actor 테이블와 film 테이블을 이용하세요.
- union, unionall, intersect, except 중 상황에 맞게 사용해주세요.
- actor_id 가 동일한 flag 값 이 여러개 나오지 않도록 해주세요.

```sql
SELECT actor_id,
       'length_over_180' AS flag
FROM   film_actor fa
WHERE  film_id IN (SELECT film_id
                   FROM   film
                   WHERE  length >= 180)
UNION
SELECT actor_id,
       'rating_R' AS flag
FROM   film_actor fa
WHERE  film_id IN (SELECT film_id
                   FROM   film
                   WHERE  rating = 'R')
```
</br>

문제2번) R등급의 영화에 출연했던 배우이면서, 동시에, Alone Trip의 영화에 출연한  영화배우의 ID 를 확인해주세요.

- film_actor 테이블와 film 테이블을 이용하세요.
- union, unionall, intersect, except 중 상황에 맞게 사용해주세요.


```sql
select distinct actor_id 
from film_actor fa
where film_id in (select film_id from film where rating = 'R')
intersect
select actor_id
from film_actor fa
where film_id in (select film_id from film where title = 'Alone Trip')

```
</br>

문제3번) G 등급에 해당하는 필름을 찍었으나,   영화를 20편이상 찍지 않은 영화배우의 ID 를 확인해주세요.

- film_actor 테이블와 film 테이블을 이용하세요.
- union, unionall, intersect, except 중 상황에 맞게 사용해주세요.

```sql
select distinct fa.actor_id 
from film f
inner join film_actor fa on f.film_id = fa.film_id
where f.rating = 'G'

except 

select distinct fa.actor_id 
from film f
inner join film_actor fa on f.film_id = fa.film_id
group by fa.actor_id 
having count(f.film_id) >= 20    
```

```sql
SELECT actor_id
FROM   film_actor fa
WHERE  film_id IN (SELECT film_id
                   FROM   film
                   WHERE  rating = 'G')
EXCEPT

SELECT actor_id
FROM   film_actor fa
GROUP  BY actor_id
HAVING Count(DISTINCT film_id) >= 20
```
굳이 서브 쿼리를 써야하나?
</br>


문제4번) 필름 중에서,  필름 카테고리가 Action, Animation, Horror 에 해당하지 않는 필름 아이디를 알려주세요.

- category 테이블을 이용해서 알려주세요.

```sql
select distinct film_id
from film_category fc 
where category_id not in 
(select category_id from category c
where name in ('A%','Horror'))                                                                                                                                                 ```

distinct를 안하면 답이 다른데 뭐가 중복된건지 이해가 안간다... 뭐가 겹쳐서 없어진거냐 ㅠㅠ
답답해 죽겠음

```sql
select film_id
from film_category fc
except
select category_id from category c
where name in ('A%','Horror')
```

film_id 와 category_id를 맞춰줘야 한다는데 잘만 나온다 이건 또 왜이런지..

</br>

~문제5번)~ Staff  의  id , 이름, 성 에 대한 데이터와 , Customer 의 id, 이름 , 성에 대한 데이터를  하나의  데이터셋의 형태로 보여주세요.

- 컬럼 구성 : id, 이름 , 성, flag (직원/고객여부) 로 구성해주세요.


```sql
SELECT staff_id,
       first_name,
       last_name,
       'Staff' AS flag
FROM   staff
UNION ALL
SELECT customer_id,
       first_name,
       last_name,
       'Customer' AS flag
FROM   customer
```


</br>

문제6번) 직원과  고객의 이름이 동일한 사람이 혹시 있나요? 있다면, 해당 사람의 이름과 성을 알려주세요.
```sql
select first_name , last_name 
from staff s 
intersect
select first_name , last_name 
from customer c


```
</br>

문제7번) 반납이 되지 않은 대여점(store)별 영화 재고 (inventory)와 전체 영화 재고를 같이 구하세요. (union all)

```sql
select null as Null , count (*)
from rental r
inner join inventory i on r.inventory_id = i.inventory_id 
where r.return_date is null
union all 
select i.store_id , count(*)
from rental r
inner join inventory i on r.rental_id = i.inventory_id 
where r.return_date is null
group by i.store_id 
```
```sql
select null as Null , count (*)
from rental r
inner join inventory i on r.inventory_id = i.inventory_id 
where r.return_date is null
union all 
select i.store_id , count(*)
from rental r
inner join inventory i on r.inventory_id = i.inventory_id 
where r.return_date is null
group by i.store_id 
```
ON 조건을 제대로 쓰자!!
</br>

문제8번) 국가(country)별 도시(city)별 매출액, 국가(country)매출액 소계 그리고 전체 매출액을 구하세요. (union all)


```sql
select c3.country, c2.city, sum(p.amount)
from payment p
inner join customer c on p.customer_id = c.customer_id
inner join address a on c.address_id = a.address_id
inner join city c2 on a.city_id = c2.city_id
inner join country c3 on c2.country_id = c3.country_id
group by c3.country , c2.city

union all 
select c3.country, null as null, sum(p.amount)
from payment p
inner join customer c on p.customer_id = c.customer_id
inner join address a on c.address_id = a.address_id
inner join city c2 on a.city_id = c2.city_id
inner join country c3 on c2.country_id = c3.country_id
group by c3.country

union all
select null as null, null as null, sum(p.amount)
from payment p
inner join customer c on p.customer_id = c.customer_id
inner join address a on c.address_id = a.address_id
inner join city c2 on a.city_id = c2.city_id
inner join country c3 on c2.country_id = c3.country_id
```
union에서 가장 중요한 점은 select 요소를 맞춰 주는 것이다.
하 겨우 끝났다 이장..
</br>
