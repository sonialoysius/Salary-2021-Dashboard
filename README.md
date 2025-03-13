use salary;
#1.Average Salary by Industry and Gender
SELECT Industry, Gender, avg(Annual_Salary) as average_salary
FROM salary_data
GROUP BY Industry, Gender
ORDER BY Industry, Gender;

#2.Total Salary Compensation by Job Title
SELECT Job_Title, SUM(Annual_Salary + Additional_Monetary_Compensation) AS Total_Compensation
FROM salary_data
GROUP BY Job_Title
ORDER BY Total_Compensation DESC;

#3.Salary Distribution by Education Level
SELECT Highest_Level_of_Education_Completed as Education_Level, 
       AVG(Annual_Salary) AS Average_Salary, 
       MIN(Annual_Salary) AS Min_Salary, 
       MAX(Annual_Salary) AS Max_Salary
FROM salary_data
GROUP BY Education_Level
ORDER BY Education_Level;

#4.Number of Employees by Industry and Years of Experience
SELECT Industry, 
       Years_of_Professional_Experience_Overall, 
       COUNT(*) AS Employee_Count
FROM salary_data
GROUP BY Industry, Years_of_Professional_Experience_Overall
ORDER BY Industry, Years_of_Professional_Experience_Overall;

#5.Median Salary by Age Range and Gender
WITH RankedSalaries AS (
    SELECT 
        Age_Range, 
        Gender, 
        Annual_Salary,
        ROW_NUMBER() OVER (PARTITION BY Age_Range, Gender ORDER BY Annual_Salary) AS RowNum,
        COUNT(*) OVER (PARTITION BY Age_Range, Gender) AS TotalCount
    FROM salary_data
)
SELECT 
    Age_Range, 
    Gender,
    AVG(Annual_Salary) AS Median_Salary
FROM RankedSalaries
WHERE RowNum IN (FLOOR((TotalCount + 1) / 2), FLOOR((TotalCount + 2) / 2))
GROUP BY Age_Range, Gender
ORDER BY Age_Range, Gender;
#6.Job Titles with the Highest Salary in Each Country
SELECT Country, Job_Title, FORMAT(MAX(Annual_Salary),0) AS Highest_Salary
FROM salary_data
GROUP BY Country, Job_Title
ORDER BY Country, Highest_Salary DESC;
select* from salary_data;
use salary;
#7.Average Salary by City and Industry
SELECT City, Industry, AVG(Annual_Salary) AS Average_Salary
FROM salary_data
GROUP BY City, Industry
ORDER BY City, Industry;

#8Percentage of Employees with Additional Monetary Compensation by Gender
SELECT Gender,
       (SUM(CASE WHEN Additional_Monetary_Compensation > 0 
       THEN 1 ELSE 0 END) / COUNT(*) * 100) AS 
       Percentage_with_Additional_Compensation
FROM salary_data
GROUP BY Gender;

#9Total Compensation by Job Title and Years of Experience
SELECT Job_Title, Years_of_Professional_Experience_Overall,
       SUM(Annual_Salary + Additional_Monetary_Compensation) AS Total_Compensation
FROM salary_data
GROUP BY Job_Title, Years_of_Professional_Experience_Overall
ORDER BY Job_Title, Years_of_Professional_Experience_Overall;

#10Average Salary by Industry, Gender, and Education Level
SELECT Industry, Gender, Highest_Level_of_Education_Completed, AVG(Annual_Salary) AS Average_Salary
FROM salary_data
GROUP BY Industry, Gender, Highest_Level_of_Education_Completed
ORDER BY Industry, Gender, Highest_Level_of_Education_Completed;
