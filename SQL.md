### mistake riddled SQL
### 1. 
select c.name, distinct sum(o.amount) as total_amt_spent
from order as o
join customer as c
on c.customer_id = o.customer_id
groupby c.name

### 2. 
with ranked_revenue as (select c.state, o.amount, rank() over (partition by c.name, order by o.amount)
as rnk from customers as c 
join orders as o
on o.customer_id = c.customer_id)
select c.state, rnk
from ranked_revenue
where rnk=2
groupby state

###
