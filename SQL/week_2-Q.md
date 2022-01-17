1.주문일이 2017-09-02 일에 해당 하는 주문건에 대해서,  어떤 고객이, 어떠한 상품에 대해서 얼마를 지불하여  상품을 구매했는지 확인해주세요.

```sql
select o.orderdate, c.custfirstname , c.custlastname ,p.productname, od.quotedprice * od.quantityordered as prices 
from orders o
join customers c on o.customerid = c.customerid
join order_details od on o.ordernumber = od.ordernumber
join products p on od.productnumber = p.productnumber 
where orderdate = '2017-09-02'
```
```sql
select o.orderdate, o.customerid, od.productnumber,
od.quotedprice * od.quantityordered as prices
from orders as o
join order_details as od on o.ordernumber = od.ordernumber
where orderdate between '2017-09-02' and '2017-09-02'
order by o.orderdate, o.customerid
```

2.헬멧을 주문한 적 없는 고객을 보여주세요.

- 헬맷은, Products 테이블의 productname 컬럼을 이용해서 확인해주세요.

```sql
select o.customerid 
from orders o 
join order_details od on o.ordernumber = od.ordernumber
join products p on od.productnumber = p.productnumber 
where p.productname not like '%Helmet%'
group by o.customerid
```
```sql
SELECT custfirstname || ' ' || custlastname AS full_name
	FROM customers c
WHERE NOT EXISTS(select *
		from orders o 
		join order_details od on o.ordernumber = od.ordernumber
		join products p on od.productnumber = p.productnumber 
		where o.customerid  = c.customerid  and p.productname like '%Helmet%'
                 )
```
이력이 없는 것을 구하려면 not exists

3.모든 제품 과 주문 일자를 나열하세요. (주문되지 않은 제품도 포함해서 보여주세요.)

```sql
select p.productname, od.ordernumber , o.orderdate 
from products p
left outer join order_details od on p.productnumber = od.productnumber 
left outer join orders o on o.ordernumber = od.ordernumber 
```

4.캘리포니아 주와 캘리포니아 주가 아닌 STATS 로 구분하여 각 주문량을 알려주세요. (CASE문 사용)

```sql
select nt.states , count(nt.ordernumber) as order_cnt
from (select c.custstate, o.ordernumber , 
		case when custstate = 'CA' then 'CA'
		else 'No_CA' 
		end as states
	from customers c
	join orders o on o.customerid = c.customerid) as nt
group by nt.states

```
에휴 섭쿼리 아직도 모르겠네

5.공급 업체 와 판매 제품 수를 나열하세요. 단 판매 제품수가 2개 이상인 곳만 보여주세요.

```sql

```

1. 가장 높은 주문 금액을 산 고객은 누구인가요?
- 주문일자별, 고객의 아이디별로, 주문번호, 주문 금액도 함께 알려주세요.

```sql

```

7.주문일자별로, 주문 갯수와,  고객수를 알려주세요.

- ex) 하루에 한 고객이 주문을 2번이상했다고 가정했을때 -> 해당의 경우는 고객수는 1명으로 계산해야합니다.

```sql

```

8번 생략

9.타이어과 헬멧을 모두 산적이 있는 고객의 ID 를 알려주세요.

- 타이어와 헬멧에 대해서는 , Products 테이블의 productname 컬럼을 이용해서 확인해주세요.

```sql

```

1. 타이어는 샀지만, 헬멧을 사지 않은 고객의 ID 를 알려주세요. Except 조건을 사용하여, 풀이 해주세요.
- 타이어, 헬멧에 대해서는, Products 테이블의 productname 컬럼을 이용해서 확인해주세요.
