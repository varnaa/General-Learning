# Custom Filters

## 'is' filter

Generic to all attributes such as activated on, last billed on, next billing on, cancelled on, expired on, created on
etc. Following examples are based on activated on.

#### Today

```sql
select *
from subscription
where activation_date = curdate();
```

#### Tomorrow

```sql
select *
from subscription
where activation_date = curdate() + INTERVAL 1 DAY -- method 1

select *
from subscription
where activation_date = adddate(curdate(), INTERVAL 1 DAY) -- using inbuilt functions
```

#### Yesterday

```sql
select *
from subscription
where activation_date = curdate() - INTERVAL 1 DAY -- method 1

select *
from subscription
where activation_date = date_sub(curdate(), INTERVAL 1 DAY) -- using inbuilt functions
```

---

### Note:

- DAYOFWEEK() - is a inbuilt function that returns an integer ranging from 1 (Sunday) to 7 (Saturday). example:

```mysql
query = selet DAYOFWEEK(curdate());
result = 2 -- 2 because monday
```

#### Starting date of this week

- Starting date and Ending date of a week can be found using the DAYOFWEEK() function

```sql
set
@day_of_week = DAYOFWEEK(curdate()) - 1;

select *
from subscription
where activation_date = date_sub(curdate(), INTERVAL @day_of_week DAY);

```

Example: Let's say today is the fourth day of the week, and we want to find the starting date of this week.

> set @day_of_week = DAYOFWEEK(curdate()); -- would return 4

Now, subtracting four days from today would gives the start of the day which is sunday.

#### Ending date of this week

- same as finding starting date of the week, instead of subtracting the days we will add the remaining days of the week.

```sql
set
@day_of_week = 7 - DAYOFWEEK(curdate());

select *
from subscription
where activation_date = date_add(curdate(), INTERVAL @day_of_week DAY);
```

---

#### Next week

```sql
set
@current_week = week(curdate()); -- gets the week number for current date

select *
from subscription
where week(curdate()) = @current_week + 1; 
```

#### Starting date of next week

Naive approach:

- Find remaining days to reach the end of current week
- +1 to that will gives us the starting date of next week

Better solution = ?

```sql
set
@days_to_starting_date = (7 - DAYOFWEEK(curdate())) + 1;

select *
from subscription
where activation_date = date_add(curdate(), INTERVAL @days_to_starting_date DAY);
```

#### Ending date of next week

```mysql
set @days_to_ending_date = (7 - DAYOFWEEK(curdate())) + 7;

select *
from subscription
where activation_date = date_add(curdate(), INTERVAL @days_to_ending_date DAY);
```

---

#### Previous week

```mysql
set @previous_week_number = week(curdate()) - 1;

select *
from subscription
where week(activation_date) = @previous_week_number;
```

#### Starting date of previous week

Naive approach: reverse of next week solution

```mysql
set @days_to_starting_date = (DAYOFWEEK(curdate()) - 1) + 7;

select *
from subcription
where activation_date = date_sub(curdate(), INTERVAL @days_to_starting_date DAY) -- subtract days from today's date
```

#### Ending date of previous week

```mysql
set @days_to_ending_date = (DAYOFWEEK(curdate());

select *
from subscription
where activation_date = date_sub(curdate(), INTERVAL @days_to_ending_date DAY);

```

---

#### This month

```sql
select *
from subscription
where
    MONTH (activation_date) = MONTH (curdate())
  and
    YEAR (activation_date) = YEAR (curdate());
```

### Note:

- LAST_DAY(date) - returns the last date of the given month
- DAY(date) / DAYOFMONTH(date) - Returns the day of the month for *`date`*, in the range `1` to `31`

Example:

```sql
select last_day(curdate()); -- returns 2021-06-30
```

```mysql
select day(curdate()); -- returns 21
```

#### Starting date of this month

```sql
select *
from subscription
where activation_date = last_day(curdate() - INTERVAL 1 MONTH) + 1 INTERVAL DAY;
```

#### Ending date of this month

```sql
select *
from subscription
where activation_date = last_day(curdate());
```

#### Next month

```sql
select *
from subscription
where
    MONTH (activation_date) = (MONTH (curdate()) + 1)
  and
    YEAR (activation_date) = YEAR (curdate());
```

#### Starting date of next month

```sql
select *
from subscription
where activation_date = last_day(curdate()) + INTERVAL 1 DAY;
```

#### Ending date of next month

```sql
select *
from subscription
where activation_date = last_day(curdate() + INTERVAL 1 MONTH) 
```

#### previous month

```sql
select *
from subscription
where
    MONTH (activation_date) = (MONTH (curdate()) - 1)
  and
    YEAR (activation_date) = YEAR (curdate());
```

#### Starting date of previous month

```sql
select *
from subscription
where activation_date = last_day(curdate() - INTERVAL 2 MONTH) + 1 INTERVAL DAY;
```

#### Ending date of previous month

```sql
select *
from subscription
where activation_date = last_day(curdate() - INTERVAL 1 MONTH);
```

