#### Syntax for storing double values:

```my'
 column_name  DECIMAL(P,D);
 
```

- P is the precision that represents the number of significant digits. The range of P is 1 to 65.
- D is the scale that that represents the number of digits after the decimal point. The range of D is 0 and 30. MySQL
  requires that D is less than or equal to (<=) P.
- Standard values for storing money is -> DECIMAL(19,4)

```sqlite
alter table subscription
    modify amount decimal (10,4);
```

```sqlite
alter table invoice
    modify total decimal (10,4),
    modify balance decimal (10,4);
modify payment_made decimal(10,4),
```

---

### MySQL date functions

#### Mysql documentation:

- CURDATE() - returns the current date
- ADDDATE(date, interval to be added to the date) - add time values to a date value
- SUBDATE(date, interval to be subtracted to the date) - subtracts time values to a date value
- NOW() - returns current date and time
- WEEK(date, mode) - returns the week number for that year. (mode ->  specifies whether the week starts on sunday or
  monday)

Example:

##### adddate()

```mysql
mysql>
SELECT ADDDATE('2008-01-02', 31);
// 31 -> interval added to the date 
        -> '2008-02-02'
```

##### subdate()

```mysql
mysql>
SELECT SUBDATE('2008-01-02 12:00:00', 31);
// 31 ->  interval subtracted to the date
        -> '2007-12-02 12:00:00'
```

##### week()

```mysql
mysql>
SELECT WEEK('2008-02-20');
-> 7
mysql>
SELECT WEEK('2008-02-20', 0);
-> 7
mysql>
SELECT WEEK('2008-02-20', 1);
-> 8
mysql>
SELECT WEEK('2008-12-31', 1);
-> 53
```

---

#### 1. Cancellations from last week:

##### Type 1:  subtracting  7 days from current date:

```sql
select *
from subscription
where status = 'cancelled'
  and cancellation_date between
    subdate(curdate(), 7) -- subract 7 days from current date
    and
    curdate();
```

##### Type 2: using the week() function and select only those records whose week number is 1 less than the week number of today's date

```sql
set
@current_week_number = week(curdate()); -- gets the week number for current date

select *
from subscription
where status = 'cancelled'
  and week(cancellation_date) = @current_week_number - 1; -- all dates falling in the week 1 less than current week number
```

---

#### 2. Amount receivable in next week

```sql
select sum(balance)
from invoice
where due_date >= adddate(curdate(), 7); -- wrong approach 


select sum(balance)
from invoice
where due_date >= adddate(curdate(), 7)
  and due_date <= adddate(curdate(), 14) -- works but not concise
---------------------------------------------------------------------------------------------------------------------------------

-- correct approach using WEEK()
    set @current_week_number = week(curdate());

select sum(balance)
from invoice
where week(due_date) = @current_week_number + 1;


```

---

### Observation:

- using ADD_DATE() or SUB_DATE() makes our query select all the records that fall after 7 days as we are limited to
  using `> or <` operators i,e : 8,9,...100th day is also fetched.
- This doesn't help when we only want a to find data for one particular week that is only 7 days.
- In such case using ` between or week() `  seems more appropriate and matches the use case.

---

#### 3. Number of customers awaiting payment in the next week:

```mysql
set @current_week_number = week(curdate());

select count(DISTINCT (customer_id))
from invoice
where balance > 0.0
  and week(due_date) = @current_week_number + 1;
```

---

#### 4. Trials expired in the previous week:

```mysql
set @previous_week_number = week(curdate()) - 1;

select *
from subscription
where status = 'trial expired'
  and week(expires_at) = @previous_week_number;
```

---

#### 5. Trials expiring in the next week:

```mysql
set @next_week_number = week(curdate()) + 1;

select *
from subscription
where status = 'trial'
  and week(expires_at) = @next_week_number;

```

---

#### 6. Trials expiring in the next seven days:

Perfect use case for using add_date()

```mysql
select *
from subscription
where status = 'trial expired'
  and expires_at between curdatae() and adddate(curdate(), 7);
```

---

#### 7. Subscription cancelled this month:

```mysql
select *
from subscription
where status = 'cancelled'
  and MONTH(cancelled_date) = MONTH(now())
  and YEAR(cancelled_date) = YEAR(now()); -- required else matches month in all years
```

---

#### 8. Subscription Cancelled Last month:

```mysql
select *
from subscription
where status = 'cancelled'
  and MONTH(cancelled_date) = MONTH(now()) - 1
  and YEAR(cancelled_date) = YEAR(now());
```

---

#### 9. Subscriptions expiring this month:

```mysql
select *
from subscription
where status = 'live'
  and MONTH(expires_at) = MONTH(now())
  and YEAR(cancelled_date) = YEAR(now());
```

---





