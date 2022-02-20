# Pewlett-Hackard-Analysis
# Analysis Overview
### We are tasked to determine the number of retiring employees per title, and identify employees who are eligible to participate in a mentorship program. This analysis will help Pewlett Hackard prepare for the “silver tsunami” as many current employees reach retirement age.

# Results
## The Number of Retiring Employees by Title

- The highest number of retirements are coming from Senior Engineers and Senior Staff
- Only two Managers are retiring

![Retiring_Titles](/Images/Retiring_Titles.png)

## The Employees Eligible for the Mentorship Program

- There's a signifact number of employees eligible for the Mentorship Program based on age criteria (specifically 1549 employess)
- Eligible employees are spread across various departments

![mentorship_eligibility](/Images/mentorship_eligibility.png)

# Summary

## How many roles will need to be filled as the "silver tsunami" begins to make an impact?
- Pewlett Hackard will have 72,458 employees nearing retirement

## Are there enough qualified, retirement-ready employees in the departments to mentor the next generation of Pewlett Hackard employees?
- There are 1,549 employess eligble for mentorship and there are 72,458 soon to retire employees
- On first glance there seems to be enough retiring employees to train mentorship eligible employees
- What we need to find out is if there is match in numbers on a department basis and on a titles basis

## We propose two new queries:
1) Count the number of mentorship ready employees and group them by department
2) Count the number of mentorship ready employees and group them by titles

## Queries:

1) 
-- Mentees count by title   
SELECT COUNT(e.emp_no), de.dept_no  
INTO mentee_by_dep  
FROM employees as e  
LEFT JOIN dept_emp as de  
ON e.emp_no = de.emp_no  
WHERE (e.birth_date BETWEEN '1965-01-01' AND '1965-12-31')  
AND de.to_date = '9999-01-01'  
GROUP BY de.dept_no  
ORDER BY de.dept_no;  

![mentees_by_title](/Images/mentees_by_title.png)

2)
-- mentorship ready  employees and their titles  
"SELECT e.emp_no,  
e.first_name,  
e.last_name,  
ti.title,  
ti.from_date,  
ti.to_date  
INTO mentee_titles  
FROM employees as e  
INNER JOIN titles AS ti  
ON (e.emp_no = ti.emp_no)  
WHERE (e.birth_date BETWEEN '1965-01-01' AND '1965-12-31')  
ORDER BY e.emp_no;  

-- Use Dictinct with Orderby to remove duplicate rows  
SELECT DISTINCT ON (mt.emp_no) mt.emp_no,  
mt.first_name,  
mt.last_name,  
mt.title  
INTO unique_mentee_titles  
FROM mentee_titles as mt  
WHERE mt.to_date = '9999-01-01'  
ORDER BY mt.emp_no ASC, mt.to_date DESC;  

--  count mentees by titles  
SELECT count(umt.title), umt.title  
INTO mentee_ready_titles  
FROM unique_mentee_titles as umt  
GROUP BY umt.title  
ORDER BY count(umt.title) DESC;"  

![mentees_by_department](/Images/mentees_by_dept.png)
