# Presence Insights: HR Attendance Dashboard (Power BI)

![Presence Insights Dashboard](images/Presence_Insights_PowerBI_Dashboard.png)

## The Story
HR recorded attendance in Excel, with one sheet per month and a short code in every cell (`P` present, `WFH` work from home, `SL` sick leave, `WO` weekly off). It was easy to record but hard to analyse: nobody could quickly say how present the team was, whether Fridays were quieter than Mondays, or when sick leave spiked.

I built this Power BI dashboard so HR can answer three questions on one page: **How present is the team? How much do people work from home? How much sick leave is taken?**

## Data
- Excel workbook with monthly attendance sheets (Apr to Jun 2022)
- 6,492 employee-day records, 99 employees, 15 attendance codes

## What I Did
1. **Power Query:** wrote a custom function that unpivots each month's sheet into rows, then combined all sheets into one `Final Data` table so new months load on refresh.
2. **DAX columns:** `WFH Count` and `SL Count` (with 0.5 for half-day codes), `Day of Week`, `Week No`, `Month Year`.
3. **DAX measures:** Total Working Days, Present Days, Presence %, WFH %, SL %.
4. **Dashboard:** KPI cards, trend charts with trend lines (daily, weekly, monthly), weekday tables, an employee table and a Name × Date attendance matrix, with Month and Date slicers.

```DAX
Total Working Days = CALCULATE(COUNTROWS('Final Data'),
    KEEPFILTERS(NOT('Final Data'[Value] IN {"WO","HO"})))
Present Days = CALCULATE(COUNTROWS('Final Data'),
    KEEPFILTERS('Final Data'[Value] IN {"P","WFH"}))
Presence % = DIVIDE([Present Days], [Total Working Days], 0)
WFH %      = DIVIDE([WFH Count], [Present Days], 0)
SL %       = DIVIDE([SL Count], [Total Working Days], 0)
```

## Challenge I Solved
After filtering out weekly offs, Presence % jumped to about **170%**. `CALCULATE` with `Value IN {"WO","HO"}` overrides the page filter, so weekly-off days were subtracted twice. Rewriting the measures with `KEEPFILTERS` fixed it. I also corrected a `Month Year` column that Power BI had read as a 2026 date, and sorted the weekly chart by week number instead of by value.

## Key Findings (1 Apr – 17 Jun 2022)
- **Presence 91.8%**, **WFH 10.0%**, **Sick leave 1.1%**
- Presence by month: April 94.0%, May 89.7%, June 91.7%
- Presence falls through the week (Monday 93.2%, Friday 90.1%) while WFH rises (Monday 8.8%, Friday 13.0%)
- Week 19 was the weakest week, at 87.0%, after a peak of 95.3% in week 18

## Tools
Power BI Desktop · Power Query (M) · DAX · Excel

## Files
- `Presence_Insights_PowerBI_Dashboard.pbix`: the report
- `images/`: dashboard screenshot
