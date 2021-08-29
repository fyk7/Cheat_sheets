```sql

-- Insert先となるDBを作成
CREATE TABLE usd_jpy_2019(
    Datetime timestamp,
    Open double precision,
    Low double precision,
    High double precision,
    Close double precision,
    Volume double precision
)
;

-- csvから上で事前に作成したDBにInsert
\copy usd_jpy_2019(Datetime, Open, Low, High, Close, Volume)
FROM
    './USDJPY.csv' WITH csv header
;

CREATE TABLE usd_jpy_min(
    Datetime timestamp,
    Open double precision,
    Low double precision,
    High double precision,
    Close double precision
)
;

INSERT INTO usd_jpy_min(
    Datetime,
    Open,
    Low,
    High,
    Close
)
SELECT DISTINCT
ON  (Datetime) Datetime,
    Open,
    Low,
    High,
    Close
FROM
    usd_jpy_2019
;

SELECT DISTINCT
ON  (c1) *
FROM
    t1
;
```
