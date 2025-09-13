Solution for Ramp Question <br>
/* Using this dataset, show the SQL query to find the rolling 3 day average transaction amount for each day in January 2021. */

WITH daily_totals AS ( <br>
    SELECT <br>
        CAST(transaction_time AS DATE) AS txn_date, <br>
        SUM(transaction_amount) AS total_amount <br>
    FROM transactions <br>
    GROUP BY CAST(transaction_time AS DATE) <br>
), <br>

<br>
rolling_avg AS ( <br>
    SELECT <br>
        txn_date, <br>
        AVG(total_amount) OVER ( <br>
            ORDER BY txn_date <br>
            ROWS BETWEEN 2 PRECEDING AND CURRENT ROW <br>
        ) AS rolling_3day_avg <br>
    FROM daily_totals <br>
) <br>
SELECT * <br>
FROM rolling_avg <br>
WHERE txn_date = '2021-01-31' <br>
