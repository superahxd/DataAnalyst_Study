문제1번) store 별로 staff는 몇명이 있는지 확인해주세요.
```sql
select store_id ,count(staff_id)
from staff s
group by store_id 
```
</br>

문제2번) 영화등급(rating) 별로 몇개 영화film을 가지고 있는지 확인해주세요.
```sql
select rating ,count(*)
from film f
group by rating
```
</br>

문제3번) 출현한 영화배우(actor)가  10명 초과한 영화명은 무엇인가요?
```sql
select f.title , count(a.first_name) as actor_cnt
from film f
join film_actor fa ON f.film_id = fa.film_id 
join actor a on fa.actor_id = a.actor_id
group by f.title
having count(a.first_name) > 10
```

```sql
select f.title , fc.cnt
from
(
select fa.film_id , count() cnt
from film_actor fa
group by fa.film_id
having count() > 10
) fc
inner join film f on f.film_id = fc.film_id
;
```

> 실행은 안되는데 지금 내가 서브쿼리가 약한 것 같다.

</br>

문제4번) 영화 배우(actor)들이 출연한 영화는 각각 몇 편인가요?

- 영화 배우의 이름 , 성 과 함께 출연 영화 수를 알려주세요.

```sql
```
</br>

문제5번) 국가(country)별 고객(customer) 는 몇명인가요?
```sql
```
</br>

문제6번) 영화 재고 (inventory) 수량이 3개 이상인 영화(film) 는?

- store는 상관 없이 확인해주세요.

```sql
```
</br>

문제7번) dvd 대여를 제일 많이한 고객 이름은?

```sql
```
</br>

문제8번) rental 테이블을  기준으로,   2005년 5월26일에 대여를 기록한 고객 중, 하루에 2번 이상 대여를 한 고객의 ID 값을 확인해주세요.
```sql
```
</br>
문제9번) film_actor 테이블을 기준으로, 출현한 영화의 수가 많은  5명의 actor_id 와 , 출현한 영화 수 를 알려주세요.
```sql
```
</br>

문제10번) payment 테이블을 기준으로,  결제일자가 2007년2월15일에 해당 하는 주문 중에서  ,  하루에 2건 이상 주문한 고객의  총 결제 금액이 10달러 이상인 고객에 대해서 알려주세요.
(고객의 id,  주문건수 , 총 결제 금액까지 알려주세요)
```sql
```
</br>

문제11번) 사용되는 언어별 영화 수는?
```sql
```
</br>

문제12번) 40편 이상 출연한 영화 배우(actor) 는 누구인가요?
```sql
```
</br>

문제13번) 고객 등급별 고객 수를 구하세요. (대여 금액 혹은 매출액  에 따라 고객 등급을 나누고 조건은 아래와 같습니다.)
/*
A 등급은 151 이상
B 등급은 101 이상 150 이하
C 등급은   51 이상 100 이하
D 등급은   50 이하

- 대여 금액의 소수점은 반올림 하세요.

HINT
반올림 하는 함수는 ROUND 입니다.	
*/
```sql
```
</br>
