# Pewlett-Hackard-Analysis.
The following are the findings from the database analysis fro Pewlett-Hackard   
The number of Individuals that are retiring are a total of 443,308.
The number of Individuals being that are being hired are 32,860 and the number of individuals available for a mentorship role are 2382.
Over all The Pewlett-Hackard members are retiring at a rapid rate and many positions will become available as a result.
 
 Code for the requested queries  
 SELECT cur.emp_no, cur.first_name, cur.last_name, cur.to_date, t.title INTO ret_titles FROM current_emp as cur RIGHT JOIN titles as t ON cur.emp_no = t.emp_no;

SELECT * FROM ret_titles

(Recent Titles)
SELECT * INTO unique_titles FROM (SELECT emp_no, first_name, last_name, to_date, title, ROW_NUMBER() OVER(PARTITION BY (first_name, last_name)ORDER BY to_date DESC) AS rn FROM ret_titles) AS tmp WHERE rn = 1 ORDER BY emp_no

SELECT * FROM unique_titles

(Aggregate Level)
SELECT COUNT(title), title INTO retiring_emp_titles FROM unique_titles GROUP BY title, to_date ORDER BY to_date DESC;

SELECT * FROM retiring_emp_titles

(Mentorship)
SELECT e.emp_no, e.first_name, e.last_name, e.birth_date, de.from_date, de.to_date, t.title INTO mentorship_info FROM employees As e INNER JOIN dept_emp as de ON de.emp_no = e.emp_no INNER JOIN titles As t ON e.emp_no = t.emp_no WHERE (de.to_date = '9999-01-01') AND(e.birth_date BETWEEN '1965-01-01' AND '1965-12-31');

SELECT * FROM mentorship_info
