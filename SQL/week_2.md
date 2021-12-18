#### 문제1번) film 테이블을 활용하여,  film 테이블의  100개의 row 만 확인해보세요.
```sql
select *
from film
limit 100
```

</br>

#### 문제2번) actor 의 성(last_name) 이  Jo 로 시작하는 사람의 id 값이 가장 낮은 사람 한사람에 대하여, 사람의  id 값과  이름, 성 을 알려주세요.
```sql
select actor_id , last_name , first_name 
from actor a
where last_name LIKE 'Jo%'
order by actor_id asc 
limit 1
```
LIKE Operator	|Description|
|----------:|:----------|
WHERE CustomerName LIKE 'a%'|	Finds any values that starts with "a"
WHERE CustomerName LIKE '%a'	|Finds any values that ends with "a"
WHERE CustomerName LIKE '%or%'	|Finds any values that have "or" in any position
WHERE CustomerName LIKE '_r%'|	Finds any values that have "r" in the second position
WHERE CustomerName LIKE 'a__%'	|Finds any values that starts with "a" and are at least 3 characters in length
WHERE ContactName LIKE 'a%o'|	Finds any values that starts with "a" and ends with "o"


</br>

#### 문제3번)film 테이블을 이용하여, film 테이블의 아이디값이 1~10 사이에 있는 모든 컬럼을 확인해주세요.
```sql
select *
from film f
where film_id between 1 and 10
```


</br>

#### 문제4번) country 테이블을 이용하여, country 이름이 A 로 시작하는 country 를 확인해주세요.
```sql
select country 
from country C
where country like 'A%'
```


</br>

#### 문제5번) country 테이블을 이용하여, country 이름이 s 로 끝나는 country 를 확인해주세요.
```sql
select country 
from country C
where country like '%s'
```


</br>

#### 문제6번) address 테이블을 이용하여, 우편번호(postal_code) 값이 77로 시작하는  주소에 대하여, address_id, address, district ,postal_code  컬럼을 확인해주세요.
```sql
select address_id , address , district , postal_code 
from address a
where postal_code like '77%'
```


</br>

#### 문제7번) address 테이블을 이용하여, 우편번호(postal_code) 값이  두번째글자가 1인 우편번호의  address_id, address, district ,postal_code  컬럼을 확인해주세요.
```sql
select address_id , address , district , postal_code 
from address a
where postal_code like '_1%'
```
```sql
select address_id, address, district ,postal_code

from address

where substring(postal_code,2,1) ='1'
```
> substring을 사용하는 방법이 있다. https://www.w3schools.com/sql/func_mysql_substring.asp
> SUBSTRING(columns , 시작 순서, 길이) ex) substring( address, 2 , 3) address 칼럼에서 2번째부터 3개째까지.

</br>

#### 문제8번) payment 테이블을 이용하여,  고객번호가 341에 해당 하는 사람이 결제를 2007년 2월 15~16일 사이에 한 모든 결제내역을 확인해주세요.
```sql
select *
from payment p
where customer_id = 341 
and date(payment_date) between '2007-02-15' and '2007-02-16' 
```


</br>

#### 문제9번) payment 테이블을 이용하여, 고객번호가 355에 해당 하는 사람의 결제 금액이 1~3원 사이에 해당하는 모든 결제 내역을 확인해주세요.
```sql
select *
from payment p
where customer_id = 355 
and amount between 1 and 3
```


</br>

#### 문제10번) customer 테이블을 이용하여, 고객의 이름이 Maria, Lisa, Mike 에 해당하는 사람의 id, 이름, 성을 확인해주세요.
```sql
select customer_id , first_name , last_name 
from customer c
where first_name = 'Maria' or 
first_name = 'Lisa' or
first_name = 'Mike'
```
```sql
select customer_id ,first_name ,last_name

from customer

where first_name  in ('Maria','Lisa','Mike')
```
> 내가 원초적으로 푼 것 같다ㅎㅎ;; 저런 방법이 있구나
> 하 근데 in 이 postgresql 에서 작동을 안한다 ㅠㅠ


</br>

#### 문제11번) film 테이블을 이용하여,  film의 길이가  100~120 에 해당하거나 또는 rental 대여기간이 3~5일에 해당하는 film 의 모든 정보를 확인해주세요.
```sql
select *
from film f 
where length between 100 and 120
or rental_duration between 3 and 5
```
>


</br>

#### 문제12번) address 테이블을 이용하여, postal_code 값이  공백('') 이거나 35200, 17886 에 해당하는 address 에 모든 정보를 확인해주세요.
```sql
select *
from address a
where postal_code = ''
or postal_code = '35200'
or postal_code = '17886'
```
```sql
select *, 
	case when postcal_code ='' 
	then 'empty' else cast(postal_code as varchar)  end as postal_code_emptyflag

from address
where postal_code  in ('', '35200','17886')
```
>내가 문제를 완전히 잘못 이해했구나 싶었지만 막상 답안을 postgre로 실행시켜보니 답이 안나온다..
>in이 다 작동 안함. 


</br>

#### ~문제13번)~ address 테이블을 이용하여,  address 의 상세주소(=address2) 값이  존재하지 않는 모든 데이터를 확인하여 주세요.
```sql
select *
from address a
where address2 = ''
```
> 아무래도 공백을 찾은 것은 답이 아니었던 것 같다. null로 찾아야함.
```sql
select *,  
coalesce(address2,null) as new_address2
from address
where address2 is null
```
```sql
select *
from address
where address2 is null
```
> coalesce를 빼고 해도 똑같은 답이 나오는데 굳이 칼럼을 추가할 필요가 있을까?
> coalesce  https://www.w3schools.com/sql/func_sqlserver_coalesce.asp
> 쿼리 사용시 해당 컬럼의 기본값이 Null일 경우 다른 값으로 변경해서 사용해야하는 경우가 있다. 그럴때 사용하는 것이 COALESCE() 함수 이다.
> COALESCE(columns, 'empty') => column의 null을 empty로 바꿔준다

</br>

#### ~문제14번)~ staff 테이블을 이용하여, staff 의  picture  사진의 값이 있는  직원의  id, 이름,성을 확인해주세요.  단 이름과 성을  하나의 컬럼으로 이름, 성의형태로  새로운 컬럼 name 컬럼으로 도출해주세요.
```sql
select staff_id , first_name ||', '|| last_name as name 
from staff s
where picture != ''
```
```sql
select staff_id , first_name ||', '|| last_name as name 
from staff s
where picture notnull 
```


</br>

#### 문제15번) rental 테이블을 이용하여,  대여는했으나 아직 반납 기록이 없는 대여건의 모든 정보를 확인해주세요.
```sql
select *
from rental r
where rental_date notnull
and return_date is null
```
> timestamp 칼럼에는 != '' 를 사용할 수 없다. is null, notnull로 기록을 찾아야 한다.
```sql
select *
from rental
where return_date  is null
```
> 빌린 목록이니까 굳이 rental_date 가 비었는지 확인 할 필요는 없나보다.

</br>

#### ~문제16번)~ address 테이블을 이용하여, postal_code 값이  빈 값(NULL) 이거나 35200, 17886 에 해당하는 address 에 모든 정보를 확인해주세요.
```sql
select *
from address a
where postal_code is null 
or postal_code = '35200'
or postal_code = '17886'
```
> 12번과 비슷한데 나오는 테이블이 다르다.
```sql
select *
from address
where postal_code  in ('35200','17886') or postal_code is null
```
> 문제 이해부터 잘못했다 ^^; 여전히 in은 작동안한다. 디비버가 문젠가.

</br>

#### 문제17번) 고객의 성에 John 이라는 단어가 들어가는, 고객의 이름과 성을 모두 찾아주세요.
```sql
select *
from customer c 
where last_name like '%John%'
```

</br>

#### 문제18번) 주소 테이블에서, address2 값이 null 값인 row 전체를 확인해볼까요?
```sql
select *
from address a 
where address2 is null 
```
</br>
