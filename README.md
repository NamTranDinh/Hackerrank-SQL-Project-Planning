# Hackerrank-SQL-Project-Planning
SQL Server Solution without using joins

SELECT CASE WHEN diff_rn >1 then DATEADD(day,-(diff_rn-1),start_date)
ELSE start_date
END as start_date
,end_date
FROM
(select start_date
 ,
 end_date
 ,
 diff
 ,
 rn
 ,
 rn-LAG(rn,1,0) OVER (ORDER BY rn) AS diff_rn
FROM
(SELECT 
start_date
 ,
 end_date
 ,
 DATEDIFF(day,end_date,LEAD(start_date,1,start_date) OVER (ORDER BY start_date)) AS diff
,
ROW_NUMBER() OVER (ORDER BY start_date) AS rn

FROM Projects) AS sub_source
WHERE diff <>0
) AS main_source
ORDER BY diff_rn,start_date
