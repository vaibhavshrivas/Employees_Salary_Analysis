# Employees Salary Analysis

## Here's the analysis of emoloyee's salary with their respective department.
📂 Dataset Tables Used\
`Employees Table`: Contains all the Employees and their Salary details.\
`Department Table`:Contails department of each employee.\

🔴Query to print 3rd highest salaried employee details for each department .
```sql
select * from 
(select *, 
rank() over (partition by dept_id order by salary desc) rn
from employee 
) A
where rn =3
```
### Sample Output:
<img src="https://github.com/user-attachments/assets/6647d862-b3c0-4821-8c8d-3cceb162681c"
width="520" height="55" alt="image" />

🔴Query to print 3rd highest salaried employee details for each department (give preferece to younger employee in case of a tie).in case a department has less than 3 employees then print the details of highest salaried employee in that department.
```sql
with ran_cte as
(
select *,
RANK() over (partition by dept_id order by salary desc,emp_age) rn
from employee
)
,cnt as 
(select dept_id,count(*) d
from employee
group by dept_id
)
select * from ran_cte
inner join cnt on ran_cte.dept_id=cnt.dept_id
where rn =3 or (rn=1 and d<3)
```

### Sample Output
<img src="https://github.com/user-attachments/assets/dd8bf1bb-4102-4bfb-b2af-0efbfdd7e054"
  width="590" height="109" alt="image" />

🔴Query to Print emp name , their manager name and diffrence in their age (in days) for employees whose year of birth is before their managers year of birth
```sql
select e.emp_name,e.dob emp_dob,m.emp_name manager_name,m.dob Manager_dob,DATEDIFF(day,e.dob,m.dob) date_diff
from employee e
inner join employee m on e.manager_id=m.emp_id
where m.dob>e.dob
```
### Sample Output:
<img src="https://github.com/user-attachments/assets/a8497d5d-c396-4289-8944-ec3959c25db5"
  width="427" height="124" alt="image" />

🔴Print manager names along with the comma separated list(order by emp salary) of all employees directly reporting to him.

```sql
select m.emp_name manager_name,STRING_AGG(e.emp_name,',') E_name
from employee e
inner join employee m on e.manager_id=m.emp_id
group by m.emp_name
```
### Sample Output:
<img src="https://github.com/user-attachments/assets/f3cf85df-a88d-4e3f-be9c-a0bf574a7c2c"
  width="276" height="106" alt="image" />


🔴Print dep name and average salary of employees in that dep
```sql
select d.dep_name,AVG(salary) Avg_Salary
from employee e
inner join dept d on e.dept_id=d.dep_id
group by d.dep_name
order by d.dep_name
```
### Sample Output:
<img src="https://github.com/user-attachments/assets/18d88d1e-a92f-4690-97d8-1c9f82dfe12f"
  width="163" height="94" alt="image" />

>Tool Used:\
>Microsoft Excel,\
>SQL,\
>SQL Server RDBMS
















