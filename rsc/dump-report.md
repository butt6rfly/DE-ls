-- =========================================================
-- Questions:
-- =========================================================

-- 1. List all students

SELECT * FROM students

-- 2. Show students and their departments

SELECT s.student_number, s.full_name, d.name
FROM students s INNER JOIN departments d 
ON s.department_id = d.id

-- 3. Count students by status

SELECT status, count(*) as students_count
FROM students 
GROUP BY status

-- 4. List courses with department name

SELECT c.code, c.title as course, c.credits, c.department_id, d.name 
from courses c INNER JOIN departments d 
ON c.department_id = d.id

-- 5. Show enrollments with student and course

SELECT s.student_number, s.full_name, c.title as course, e.enrolled_on, e.final_status
FROM enrollments e 
INNER JOIN students s ON e.student_id = s.id
INNER JOIN courses c ON e.offering_id = c.id

-- 6. Average grade by course offering

SELECT c.title as course_offering, round(sum(g.grade)/count(g.grade),2) as average_grade
FROM course_offerings co
INNER JOIN courses c ON co.id = c.id
INNER JOIN assignments a ON c.id = a.offering_id
INNER JOIN grades g ON a.id = g.assignment_id
GROUP BY c.id

-- 7. Students with no grades yet

SELECT s.full_name, g.grade
FROM students s
LEFT JOIN grades g ON s.id = g.student_id
WHERE grade IS NULL

-- 8. Courses with more than 2 enrolled students

SELECT c.title, COUNT(*) AS STUDENTS
FROM enrollments e 
INNER JOIN students s ON e.student_id = s.id
INNER JOIN courses c ON e.offering_id = c.id
WHERE e.final_status = 'enrolled'
GROUP BY c.id HAVING COUNT(*) > 2