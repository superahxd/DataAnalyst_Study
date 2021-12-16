#### 문제1번) dvd 렌탈 업체의  dvd 대여가 있었던 날짜를 확인해주세요.
```sql
select distinct date(rental_date)
from rental r
```
> 왜 굳이 distant를 쓰지? > 중복제거를 해준 것.
</br>

#### 문제2번) 영화길이가 120분 이상이면서, 대여기간이 4일 이상이 가능한, 영화제목을 알려주세요.

```sql
select title
from film f
where length >= 120 and rental_duration >= 4
```
</br>


#### 문제3번) 직원의 id 가 2 번인  직원의  id, 이름, 성을 알려주세요
```sql
select staff_id , first_name , last_name 
from staff s
where staff_id = 2
```
</br>

#### 문제4번) 지불 내역 중에서,   지불 내역 번호가 17510 에 해당하는  ,  고객의 지출 내역 (amount ) 는 얼마인가요?
```sql
select amount
from payment p
where payment_id = 17510
```
</br>

#### 문제5번) 영화 카테고리 중에서 ,Sci-Fi  카테고리의  카테고리 번호는 몇번인가요?
```SQL
select category_id 
from category c 
where name = 'Sci-Fi'

```
</br>


#### 문제6번) film 테이블을 활용하여, rating  등급(?) 에 대해서, 몇개의 등급이 있는지 확인해보세요.

```sql
select count(distinct rating)
from film f

```
</br>

#### 문제7번) 대여 기간이 (회수 - 대여일) 10일 이상이였던 rental 테이블에 대한 모든 정보를 알려주세요.
단 , 대여기간은  대여일자부터 대여기간으로 포함하여 계산합니다.

```sql
select * , date(return_date) - date(rental_date) + 1 as diff_date
from rental r 
where date(return_date) - date(rental_date) + 1 >= 10  

```

> 대여기간 구할 때 +1 잊지말기

</br>


#### 문제8번) 고객의 id 가  50,100,150 ..등 50번의 배수에 해당하는 고객들에 대해서,
회원 가입 감사 이벤트를 진행하려고합니다.
고객 아이디가 50번 배수인 아이디와, 고객의 이름 (성, 이름)과 이메일에 대해서
확인해주세요.

```sql
select customer_id , concat(first_name, last_name), email 
from customer c
where customer_id % 50 = 0
```
```sql
select customer_id, last_name, first_name, email
from customer
where mod(customer_id,50) =0
```
</br>

21-12-16

#### 문제9번) 영화 제목의 길이가 8글자인, 영화 제목 리스트를 나열해주세요.

```sql
select title
from film f 
where length (title) = 8
```
</br>


#### 문제10번)	city 테이블의 city 갯수는 몇개인가요?

```sql
select count(city)
from city c 
```
</br>

#### 문제11번)	영화배우의 이름 (이름+' '+성) 에 대해서,  대문자로 이름을 보여주세요.  단 고객의 이름이 동일한 사람이 있다면,  중복 제거하고, 알려주세요.

```sql
select upper(first_name || ' ' || last_name) 
from actor a 
```
</br>
> ||  : null과 합치면 null 나옴, concat : null 무시

영화배우 이름 얘기하는데 고객 이름이 갑자기 뭔 소릴까.. 조인해서 고객이름 과 같은 이름을 빼라는건가?

#### 문제12번)	고객 중에서,  active 상태가 0 인 즉 현재 사용하지 않고 있는 고객의 수를 알려주세요.

```sql
select count(*)
from customer c
where active = 0
```
</br>

#### 문제13번)	Customer 테이블을 활용하여,  store_id = 1 에 매핑된  고객의 수는 몇명인지 확인해보세요.

```sql
select count(*)
from customer c
where store_id = 1
```
</br>

#### 문제14번)	rental 테이블을 활용하여,  고객이 return 했던 날짜가 2005년6월20일에 해당했던 rental 의 갯수가 몇개였는지 확인해보세요.

```sql
select count(*)
from rental r
where date(rental_date)='2005-06-20'
```
</br>

#### 문제15번)	film 테이블을 활용하여, 2006년에 출시가 되고 rating 이 'G' 등급에 해당하며, 대여기간이 3일에 해당하는  것에 대한 film 테이블의 모든 컬럼을 알려주세요.

```sql
select *
from film f
where release_year = 2006 and rating = 'G' and rental_duration=3 
```
</br>

#### 문제16번)	langugage 테이블에 있는 id, name 컬럼을 확인해보세요 .

```sql
select language_id , name
from "language" l 
```
</br>

#### 문제17번)	film 테이블을 활용하여,  rental_duration 이  7일 이상 대여가 가능한  film 에 대해서  film_id,   title,  description 컬럼을 확인해보세요.

```sql
select film_id , title , description 
from film f 
where rental_duration >= 7
```
</br>

#### 문제18번)	film 테이블을 활용하여,  rental_duration   대여가 가능한 일자가 3일 또는 5일에 해당하는  film_id,  title, desciption 을 확인해주세요.

```sql
select film_id , title , description 
from film f 
where rental_duration >= 7
```
</br>

#### 문제19번)	Actor 테이블을 이용하여,  이름이 Nick 이거나  성이 Hunt 인  배우의  id 와  이름, 성을 확인해주세요.

```sql
select actor_id , first_name , last_name 
from actor a
where first_name = 'Nick' or last_name = 'Hunt'
```
</br>

#### 문제20번)	Actor 테이블을 이용하여, Actor 테이블의  first_name 컬럼과 last_name 컬럼을 , firstname, lastname 으로 컬럼명을 바꿔서 보여주세요

```sql
select first_name as firstname , last_name as lastname 
from actor a
```
</br>
