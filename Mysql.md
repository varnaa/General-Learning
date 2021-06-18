# DBMS

## Tables:

1. Customer
2. Subscription
3. Invoice

---

### Queries

#### Create Database

```sql
create
database mydb;
```

#### 1. Customer Table:

```sql
create table customer
(
    customer_id   int,
    customer_name varchar(255),
    primary key (customer_id)
);
```

```sql
insert into customer(id, name)
values (1, "varnaa");
insert into customer(id, name)
values (2, "varshaa");
insert into customer(id, name)
values (3, "kishore");
insert into customer(id, name)
values (4, "sai");
insert into customer(id, name)
values (5, "meha");
```

![customer.png](https://github.com/varnaa/General-Learning/blob/master/screenshots/customer.png?raw=true)

#### 2. Invoice Table:

```sql
create table invoice
(
    invoice_id   int,
    status       varchar(255),
    total        int,
    payment_made int,
    balance      int,
    invoice_date date,
    due_date     date,
    customer_id  int,
    primary key (invoice_id),
    foreign key (customer_id) references customer (customer_id)
);
```

```sql
INSERT INTO mydb.invoice (invoice_id, status, total, payment_made, balance, invoice_date, due_date, customer_id)
VALUES (3, 'sent', 700, 0, 700, '2021-06-10', '2021-06-15', 3);

INSERT INTO mydb.invoice (invoice_id, status, total, payment_made, balance, invoice_date, due_date, customer_id)
VALUES (4, 'paid', 100, 100, 0, '2021-06-17', '2021-06-17', 1);

INSERT INTO mydb.invoice (invoice_id, status, total, payment_made, balance, invoice_date, due_date, customer_id)
VALUES (5, 'sent', 600, 0, 600, '2021-06-15', '2021-06-16', 2);

INSERT INTO mydb.invoice (invoice_id, status, total, payment_made, balance, invoice_date, due_date, customer_id)
VALUES (6, 'sent', 100, 0, 100, '2021-06-07', '2021-06-14', 3);

INSERT INTO mydb.invoice (invoice_id, status, total, payment_made, balance, invoice_date, due_date, customer_id)
VALUES (7, 'sent', 850, 0, 850, '2021-06-05', '2021-06-15', 4);

INSERT INTO mydb.invoice (invoice_id, status, total, payment_made, balance, invoice_date, due_date, customer_id)
VALUES (8, 'sent', 670, 0, 670, '2021-06-07', '2021-06-11', 5);

INSERT INTO mydb.invoice (invoice_id, status, total, payment_made, balance, invoice_date, due_date, customer_id)
VALUES (9, 'paid', 500, 500, 0, '2021-06-15', '2021-06-15', 5);

INSERT INTO mydb.invoice (invoice_id, status, total, payment_made, balance, invoice_date, due_date, customer_id)
VALUES (10, 'paid', 499, 499, 0, '2021-06-10', '2021-06-10', 3);

INSERT INTO mydb.invoice (invoice_id, status, total, payment_made, balance, invoice_date, due_date, customer_id)
VALUES (11, 'paid', 999, 999, 0, '2021-06-09', '2021-06-10', 1);

INSERT INTO mydb.invoice (invoice_id, status, total, payment_made, balance, invoice_date, due_date, customer_id)
VALUES (12, 'paid', 499, 499, 0, '2021-06-10', '2021-06-09', 2);

INSERT INTO mydb.invoice (invoice_id, status, total, payment_made, balance, invoice_date, due_date, customer_id)
VALUES (13, 'paid', 499, 499, 0, '2021-06-10', '2021-06-10', 1);

INSERT INTO mydb.invoice (invoice_id, status, total, payment_made, balance, invoice_date, due_date, customer_id)
VALUES (14, 'paid', 499, 499, 0, '2021-06-11', '2021-06-11', 3);

INSERT INTO mydb.invoice (invoice_id, status, total, payment_made, balance, invoice_date, due_date, customer_id)
VALUES (15, 'paid', 999, 999, 0, '2021-06-08', '2021-06-12', 4);

```

![invoice.png](https://github.com/varnaa/General-Learning/blob/master/screenshots/invoice.png?raw=true)

#### 3. Subscription Table:

```sql
create table subscription
(
    subscription_id int,
    plan_code       varchar(255),
    status          varchar(255),
    amount          int,
    activated_at    DATE,
    expires_at      DATE,
    customer_id     int,
    invoice_id      int,
    primary key (subscription_id),
    foreign key (customer_id) references customer (customer_id),
    foreign key (invoice_id) references invoice (invoice_id)
);
```

```sql
INSERT INTO mydb.subscription (subscription_id, plan_code, status, activated_at, expires_at, customer_id, invoice_id,
                               amount)
VALUES (6, 'premium', 'active', '2021-06-04', '2021-06-10', 1, 11, 999);

INSERT INTO mydb.subscription (subscription_id, plan_code, status, activated_at, expires_at, customer_id, invoice_id,
                               amount)
VALUES (7, 'premium', 'active', '2021-06-06', '2021-06-30', 4, 15, 999);

INSERT INTO mydb.subscription (subscription_id, plan_code, status, activated_at, expires_at, customer_id, invoice_id,
                               amount)
VALUES (8, 'elite', 'active', '2021-06-18', '2021-06-30', 1, 19, 999);

INSERT INTO mydb.subscription (subscription_id, plan_code, status, activated_at, expires_at, customer_id, invoice_id,
                               amount)
VALUES (9, 'basic', 'active', '2021-06-03', '2021-06-11', 4, 7, 499);

INSERT INTO mydb.subscription (subscription_id, plan_code, status, activated_at, expires_at, customer_id, invoice_id,
                               amount)
VALUES (10, 'basic', 'active', '2021-06-01', '2021-06-30', 3, 3, 499);

INSERT INTO mydb.subscription (subscription_id, plan_code, status, activated_at, expires_at, customer_id, invoice_id,
                               amount)
VALUES (11, 'preimum', 'cancelled', '2021-06-01', '2021-06-30', 4, 7, 999);
```

![subscription.png](https://github.com/varnaa/General-Learning/blob/master/screenshots/subscription.png?raw=true)

---

## Questions:

#### 1. invoice count of customers whose payment is overdue:

**Method 1**:  Directly selecting the invoices by filtering the due date and balance to paid.

```sql
select count(*)
from invoice
where due_date < CURDATE()
  and invoice.balance > 0;
```

**Method 2**:

	- Step 1: update the status of the invoices to overdue
	- Step 2: select the invoices whose status is overdue based on the balance.

```sql
// Step 1
update invoice
set status = 'overdue'
where due_date > CURDATE()
  and invoice.balance > 0;

// Step 2
select count(*)
from invoice
where status = 'overdue';
```

![qn1.png](https://github.com/varnaa/General-Learning/blob/master/screenshots/qn1.png?raw=true)

---

#### 2. Find if there is any balance to be paid by a customer:

```sql
select invoice_id, status, total, payment_made, balance, c.customer_name, c.customer_id
from invoice
         join customer c on c.customer_id = invoice.customer_id
where balance > 0;
```

![qn2.png](https://github.com/varnaa/General-Learning/blob/master/screenshots/qn2.png?raw=true)

---

#### 3. Amount receivable in next week:

```sql
select sum(balance)
from invoice
where due_date >= '2020-06-07';
```

![qn3.png](https://github.com/varnaa/General-Learning/blob/master/screenshots/qn3.png?raw=true)

---

#### 4. Number of customers awaiting payment in the next week:

```sql
select count(DISTINCT (customer_id))
from invoice
where balance > 0
  and due_date >= '2021-06-17';

// or can be filtered through status instead of balance

select count(DISTINCT (customer_id))
from invoice
where status = 'sent'
  and due_date >= '2021-06-17';
```

![qn4.png](https://github.com/varnaa/General-Learning/blob/master/screenshots/qn4.png?raw=true)



---

#### 5. Number of customers in trial subscription:

```sql
select count(*)
from subscription
where status = 'trial';
```

![qn5.png](https://github.com/varnaa/General-Learning/blob/master/screenshots/qn5.png?raw=true)

---

#### 6. cancellations from last week:

```sql
// count(*) can be used for no: of cancellations

select *
from subscription
where status = 'cancelled'
  and cancellation_date >= '2021-06-10'
  and subscription.cancellation_date < curdate();
```

![qn6.png](https://github.com/varnaa/General-Learning/blob/master/screenshots/qn6.png?raw=true)

---

#### 7. Most popular plan based on the revenue generated:

**Case 1** : Plans with fixed price

Get the total revenue generated by using the default amount specified in the subscription plan.

```sql
select plan_code, sum(amount)
from subscription
group by plan_code
order by sum(amount) desc;
// can be narrowed down to top 3 by using limit 3
```

![qn7_1.png](https://github.com/varnaa/General-Learning/blob/master/screenshots/qn7_1.png?raw=true)

**Case 2:** Plans with variable price:

Get the total revenue generated by using the total amount specified in the invoice for each plan.

```sql
select plan_code, sum(i.total)
from subscription
         join invoice i on subscription.invoice_id = i.invoice_id
group by plan_code
order by sum(i.total) desc limit 2; 
```

![qn7_2.png](https://github.com/varnaa/General-Learning/blob/master/screenshots/qn7_2.png?raw=true)



---

