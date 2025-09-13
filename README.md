Solution for Ramp Question
/* Using this dataset, show the SQL query to find the rolling 3 day average transaction amount for each day in January 2021. */

WITH daily_totals AS (
    SELECT
        CAST(transaction_time AS DATE) AS txn_date,
        SUM(transaction_amount) AS total_amount
    FROM transactions
    GROUP BY CAST(transaction_time AS DATE)
),
rolling_avg AS (
    SELECT
        txn_date,
        AVG(total_amount) OVER (
            ORDER BY txn_date
            ROWS BETWEEN 2 PRECEDING AND CURRENT ROW
        ) AS rolling_3day_avg
    FROM daily_totals
)
SELECT *
FROM rolling_avg
WHERE txn_date = '2021-01-31'
