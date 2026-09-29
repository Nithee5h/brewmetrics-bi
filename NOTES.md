# Copilot-Assisted DAX Development Notes

## Measure 1 — Month-over-Month Growth %

**Purpose:** Compare the selected month's sales with the previous month.

**Initial suggestion:**
```DAX
Previous Month Sales =
CALCULATE(
    [Total Sales],
    DATEADD(Dim_Date[Date], -1, MONTH)
)

MoM Growth % =
DIVIDE(
    [Total Sales] - [Previous Month Sales],
    [Previous Month Sales]
)
```

**Correction:** The KPI still displayed a value when several months were in filter context. I changed the measure so it only returns a value when one month is selected.

**Final formula:**
```DAX
MoM Growth % =
IF(
    HASONEVALUE(Dim_Date[Month]),
    DIVIDE(
        [Total Sales] - [Previous Month Sales],
        [Previous Month Sales]
    ),
    BLANK()
)
```

**Validation:** April is blank because March is unavailable; May is about 9.45%; June is negative because June sales are lower than May.

---

## Measure 2 — Running Total Sales

**Purpose:** Show cumulative sales across the selected reporting period.

**Initial suggestion and final formula:**
```DAX
Running Total Sales =
CALCULATE(
    [Total Sales],
    FILTER(
        ALLSELECTED(Dim_Date[Date]),
        Dim_Date[Date] <= MAX(Dim_Date[Date])
    )
)
```

**Correction:** No major formula correction was required. The main work was validating that the cumulative value increased correctly from April through June and respected report filters.

---

## Measure 3 — City Rank

**Purpose:** Rank cities by Total Sales.

**Initial suggestion:**
```DAX
City Rank =
RANKX(
    ALLSELECTED(Dim_City[City]),
    [Total Sales],
    ,
    DESC,
    DENSE
)
```

**Problem:** Every city returned rank 1.

**Correction:** I added `CALCULATE([Total Sales])` inside `RANKX` to force context transition while each city is evaluated.

**Final formula:**
```DAX
City Rank =
RANKX(
    ALLSELECTED(Dim_City[City]),
    CALCULATE([Total Sales]),
    ,
    DESC,
    DENSE
)
```

---

## Measure 4 — Average Transaction Value

**Purpose:** Calculate average revenue per transaction.

```DAX
Transaction Count =
DISTINCTCOUNT(Fact_Sales[sale_id])

Average Transaction Value =
DIVIDE(
    [Total Sales],
    [Transaction Count]
)
```

No major formula correction was needed; the measure was validated with City, Category, and Month slicers.

## Supporting Measures

```DAX
Total Sales =
SUM(Fact_Sales[sales_amount])
```

```DAX
Total Quantity =
SUM(Fact_Sales[quantity])
```

```DAX
Transaction Count =
DISTINCTCOUNT(Fact_Sales[sale_id])
```

## Summary
Copilot accelerated the first version of the DAX logic, but validation in Power BI was still necessary. The most important correction was the City Rank measure, which initially returned rank 1 for every city. The MoM measure was also improved with `HASONEVALUE` so the KPI only displays a meaningful value for a single selected month.
