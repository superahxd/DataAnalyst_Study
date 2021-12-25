문제1번) 고객의 기본 정보인, 고객 id, 이름, 성, 이메일과 함께 고객의 주소 address, district, postal_code, phone 번호를 함께 보여주세요.

```sql
select c.customer_id , c.first_name , c.last_name , a.address , a.district , a.postal_code , a.phone 
from customer c 
join address a 
on c.address_id = a.address_id 
```
</br>


문제2번) 고객의  기본 정보인, 고객 id, 이름, 성, 이메일과 함께 고객의 주소 address, district, postal_code, phone , city 를 함께 알려주세요.

```sql
select c.customer_id , c.first_name , c.last_name , a.address , a.district , a.postal_code, c2.city 
from customer c 
join address a 
on c.address_id = a.address_id
join city c2 
on a.city_id = c2.city_id 
```
</br>
문제3번) Lima City에 사는 고객의 이름과, 성, 이메일, phonenumber에 대해서 알려주세요.

```sql
select c.first_name , c.last_name , c.email , a.phone 
from customer c 
join address a 
on c.address_id = a.address_id
join city c2 
on a.city_id = c2.city_id 
where c2.city = 'Lima'

```
</br>
문제4번) rental 정보에 추가로, 고객의 이름과, 직원의 이름을 함께 보여주세요.

```sql
select r.*, 
c.first_name ||' '||c.last_name as customer_name,
s.first_name ||' '||s.last_name as staff_name
from rental r
join customer c ON r.customer_id = c.customer_id
join staff s on r.staff_id = s.staff_id
```
</br>
- 고객의 이름, 직원 이름은 이름과 성을 fullname 컬럼으로만들어서 직원이름/고객이름 2개의 컬럼으로 확인해주세요.

문제5번) [seth.hannon@sakilacustomer.org](mailto:seth.hannon@sakilacustomer.org) 이메일 주소를 가진 고객의  주소 address, address2, postal_code, phone, city 주소를 알려주세요.

```sql
select a.address , a.address2 , a.postal_code , a.phone , c2.city 
from customer c 
join address a ON c.address_id = a.address_id
join city c2 on a.city_id = c2.city_id
where c.email = 'seth.hannon@sakilacustomer.org'

```
</br>
문제6번) Jon Stephens 직원을 통해 dvd대여를 한 payment 기록 정보를  확인하려고 합니다.
- payment_id,  고객 이름 과 성,  rental_id, amount, staff 이름과 성을 알려주세요.

```sql
select p.payment_id ,
c.first_name || ' ' || c.last_name as customer_name,
p.rental_id , p.amount ,
s.first_name || ' ' || s.last_name as staff_name
from staff s 
join payment p on p.staff_id = s.staff_id
join customer c on p.customer_id = c.customer_id 

where s.first_name || ' ' || s.last_name  = 'Jon Stephens'

```
</br>
문제7번) 배우가 출연하지 않는 영화의 film_id, title, release_year, rental_rate, length 를 알려주세요.

```sql
select f.film_id , f.title , f.release_year , f.rental_rate , f.length 
from film f
left outer join film_actor fa on fa.film_id = f.film_id 
left outer join actor a on fa.actor_id = a.actor_id
where a.actor_id is null
```
none 아니고 null!
</br>
문제8번) store 상점 id별 주소 (address, address2, distict) 와 해당 상점이 위치한 city 주소를 알려주세요.

```sql
select s.address_id , a.address , a.address2 , a.district , c.city 
from store s
join address a on s.address_id = a.address_id 
join city c on a.city_id = c.city_id 

```
</br>
문제9번) 고객의 id 별로 고객의 이름 (first_name, last_name), 이메일, 고객의 주소 (address, district), phone번호, city, country 를 알려주세요.

```sql
select c.customer_id , c.first_name ||', '|| c.last_name as name , c.email, a.address ||', '|| a.district as address, a.phone , c2.city , c3.country 
from customer c 
join address a on c.address_id = a.address_id 
join city c2 on a.city_id = c2.city_id 
join country c3 on c2.country_id = c3.country_id  
```
</br>
문제10번) country 가 china 가 아닌 지역에 사는, 고객의 이름(first_name, last_name)과 , email, phonenumber, country, city 를 알려주세요

```sql
select c.first_name , c.last_name , c.email , a.phone , c3.country , c2.city 
from customer c 
join address a on c.address_id = a.address_id 
join city c2 on a.city_id = c2.city_id 
join country c3 on c2.country_id = c3.country_id 
where c3.country != 'China'
```
</br>
문제11번) Horror 카테고리 장르에 해당하는 영화의 이름과 description 에 대해서 알려주세요
```sql
select f.title , f.description 
from category c 
join film_category fc on fc.category_id = c.category_id
join film f on fc.film_id = f.film_id 
where c."name" = 'Horror'
```
</br>
문제12번) Music 장르이면서, 영화길이가 60~180분 사이에 해당하는 영화의 title, description, length 를 알려주세요.

- 영화 길이가 짧은 순으로 정렬해서 알려주세요.

```sql
from category c 
join film_category fc on fc.category_id = c.category_id
join film f on fc.film_id = f.film_id 
where c."name" = 'Music' and f.length between 60 and 180
order by f.length asc
```
</br>
~문제13번)~ actor 테이블을 이용하여,  배우의 ID, 이름, 성 컬럼에 추가로    'Angels Life' 영화에 나온 영화 배우 여부를 Y , N 으로 컬럼을 추가 표기해주세요.  해당 컬럼은 angelslife_flag로 만들어주세요.

```sql
select a.actor_id , a.first_name , a.last_name ,
case 	
	when f.title = 'Angels Life' then 'Y'
	else 'N'
end as angelslife_flag
from actor a 
join film_actor fa on a.actor_id = fa.actor_id 
join film f on fa.film_id = f.film_id;
```
> 제대로 된 답이 안나옴

```sql
select a.actor_id , a.first_name, a.last_name , case when a.actor_id in (
		select actor_id
		  from film f
		 inner join film_actor fa 
			on f.film_id  = fa.film_id
		 where f.title ='Angels Life') then 'Y'
		 else 'N'
		 end as angelslife_flag
  from actor a;
```
> 서브쿼리 넣는 거에 아직 안 익숙해서 연습이 필요할 듯
</br>
문제14번) 대여일자가 2005-06-01~ 14일에 해당하는 주문 중에서 , 직원의 이름(이름 성) = 'Mike Hillyer' 이거나  고객의 이름이 (이름 성) ='Gloria Cook'  에 해당 하는 rental 의 모든 정보를 알려주세요.

- 추가로 직원이름과, 고객이름에 대해서도 fullname 으로 구성해서 알려주세요.

```sql
select r.*, s.first_name ||' '||s.last_name as staff_name , c.first_name ||' '||c.last_name as customer_name
from rental r
join staff s on r.staff_id = s.staff_id
join customer c on r.customer_id = c.customer_id 
where date(r.rental_date) between '2005-06-01' and '2005-06-14'
and (s.first_name ||' '||s.last_name = 'Mike Hillyer'
or c.first_name ||' '||c.last_name = 'Gloria Cook')
```
> 정답이랑 좀 다른데 조회는 똑같이 됨

```sql
select r.*,
c.first_name , c.last_name ,
s.first_name , s.last_name
from rental as r 	
left outer join customer as  c on r.customer_id  =  c.customer_id
left outer join staff s on r.staff_id  = s.staff_id
where
( s.first_name || ' ' ||s.last_name ='Mike Hillyer'
or c.first_name  || ' '|| c.last_name  ='Gloria Cook'
)
and  date(rental_date) between '2005-06-01' and '2005-06-14'
```

</br>
문제15번) 대여일자가 2005-06-01~ 14일에 해당하는 주문 중에서 , 직원의 이름(이름 성) = 'Mike Hillyer' 에 해당 하는 직원에게  구매하지 않은  rental 의 모든 정보를 알려주세요.

- 추가로 직원이름과, 고객이름에 대해서도 fullname 으로 구성해서 알려주세요.

```sql
select r.*,s.first_name ||' '||s.last_name as staff_name , c.first_name ||' '||c.last_name as customer_name
from rental as r 	
left outer join customer as  c on r.customer_id  =  c.customer_id
left outer join staff s on r.staff_id  = s.staff_id
where s.first_name || ' ' ||s.last_name !='Mike Hillyer'
and  date(rental_date) between '2005-06-01' and '2005-06-14'
```

```sql
select r.*,
c.first_name , c.last_name ,
s.first_name , s.last_name
from rental as r 	
left outer join customer as  c on r.customer_id  =  c.customer_id
left outer join staff s on r.staff_id  = s.staff_id
where
s.first_name || ' ' ||s.last_name not in ('Mike Hillyer')
and  date(rental_date) between '2005-06-01' and '2005-06-14'
```
</br>
