# Calculated Fields Documentation -- Sales KPI Dashboard

This document provides a complete description of all calculated fields
used in the Tableau Sales KPI Dashboard. Each calculation is documented
with its purpose, logic, and business relevance to ensure transparency
and recruiter-friendly understanding.

------------------------------------------------------------------------

## Min Max Profit

**Type:** Continuous Measure\
**Technique Used:** Table Calculation (WINDOW_MIN / WINDOW_MAX)

**Purpose:**\
Identifies the minimum and maximum profit values within the selected
window for highlighting extreme performance points.

**Calculation:**

``` sql
IF SUM([Total CY Profit]) = WINDOW_MAX(SUM([Total CY Profit])) THEN SUM([Total CY Profit])
ELSEIF SUM([Total CY Profit]) = WINDOW_MIN(SUM([Total CY Profit])) THEN SUM([Total CY Profit])
ELSE NULL
END
```

**Business Impact:**\
Helps management quickly spot best and worst performing profit periods.

------------------------------------------------------------------------

## Min Max Qty

**Type:** Continuous Measure\
**Technique Used:** Table Calculation

**Purpose:**\
Highlights the highest and lowest quantity sold within the analysis
window.

**Calculation:**

``` sql
IF SUM([Total CY Qty]) = WINDOW_MAX(SUM([Total CY Qty])) THEN SUM([Total CY Qty])
ELSEIF SUM([Total CY Qty]) = WINDOW_MIN(SUM([Total CY Qty])) THEN SUM([Total CY Qty])
ELSE NULL
END
```

**Business Impact:**\
Supports demand analysis and inventory planning decisions.

------------------------------------------------------------------------

## Min Max Sales

**Type:** Continuous Measure\
**Technique Used:** Table Calculation

**Purpose:**\
Identifies peak and lowest sales values for performance trend analysis.

**Calculation:**

``` sql
IF SUM([Total CY sales]) = WINDOW_MAX(SUM([Total CY sales])) THEN SUM([Total CY sales])
ELSEIF SUM([Total CY sales]) = WINDOW_MIN(SUM([Total CY sales])) THEN SUM([Total CY sales])
ELSE NULL
END
```

**Business Impact:**\
Helps track sales volatility and seasonal spikes.

------------------------------------------------------------------------

## Avg Profit State Wise

**Type:** Continuous Measure\
**Technique Used:** FIXED Level of Detail (LOD)

**Purpose:**\
Calculates average profit per state, independent of view-level filters.

**Calculation:**

``` sql
{ FIXED [State/Province] : AVG([Total CY Profit]) }
```

**Business Impact:**\
Used to compare state-level profitability consistently.

------------------------------------------------------------------------

## Avg Sales State Wise

**Type:** Continuous Measure\
**Technique Used:** FIXED Level of Detail (LOD)

**Purpose:**\
Computes average sales per state for geographic performance comparison.

**Calculation:**

``` sql
{ FIXED [State/Province] : AVG([Total CY sales]) }
```

**Business Impact:**\
Helps identify high and low performing regions.

------------------------------------------------------------------------

## Dynamic Measure

**Type:** Continuous Measure\
**Technique Used:** Parameter-driven logic

**Purpose:**\
Allows users to dynamically switch between Sales, Profit, and Quantity
metrics.

**Calculation:**

``` sql
CASE [Segment Measure]
WHEN 'Total CY Sales' THEN [Total CY sales]
WHEN 'Total CY Profit' THEN [Total CY Profit]
WHEN 'Total CY Qty' THEN [Total CY Qty]
END
```

**Business Impact:**\
Enables interactive dashboards and flexible KPI analysis.

------------------------------------------------------------------------

## Overall Avg Profit

**Type:** Continuous Measure\
**Technique Used:** FIXED LOD with Date Logic

**Purpose:**\
Calculates overall average profit for the current year.

**Calculation:**

``` sql
{ FIXED : AVG(
IF YEAR([Order Date]) = { MAX(YEAR([Order Date])) }
THEN [Total CY Profit]
END) }
```

**Business Impact:**\
Acts as a benchmark for profit performance comparison.

------------------------------------------------------------------------

## Overall Avg Sales

**Type:** Continuous Measure\
**Technique Used:** FIXED LOD with Date Logic

**Purpose:**\
Calculates overall average sales for the current year.

**Calculation:**

``` sql
{ FIXED : AVG(
IF YEAR([Order Date]) = { MAX(YEAR([Order Date])) }
THEN [Total CY sales]
END) }
```

**Business Impact:**\
Used to benchmark sales performance across states and segments.

------------------------------------------------------------------------

## Total CY Profit

**Type:** Continuous Measure

**Purpose:**\
Calculates total profit for the current year.

**Calculation:**

``` sql
IF YEAR([Order Date]) = { MAX(YEAR([Order Date])) }
THEN [Profit]
END
```

------------------------------------------------------------------------

## Total CY Qty

**Type:** Continuous Measure

**Purpose:**\
Calculates total quantity sold for the current year.

**Calculation:**

``` sql
IF YEAR([Order Date]) = { MAX(YEAR([Order Date])) }
THEN [Quantity]
END
```

------------------------------------------------------------------------

## Total CY Sales

**Type:** Continuous Measure

**Purpose:**\
Calculates total sales for the current year.

**Calculation:**

``` sql
IF YEAR([Order Date]) = { MAX(YEAR([Order Date])) }
THEN [Sales]
END
```

------------------------------------------------------------------------

## Total PY Profit

**Type:** Continuous Measure

**Purpose:**\
Calculates total profit for the previous year.

**Calculation:**

``` sql
IF YEAR([Order Date]) = { MAX(YEAR([Order Date])) } - 1
THEN [Profit]
END
```

------------------------------------------------------------------------

## Total PY Qty

**Type:** Continuous Measure

**Purpose:**\
Calculates total quantity sold for the previous year.

**Calculation:**

``` sql
IF YEAR([Order Date]) = { MAX(YEAR([Order Date])) } - 1
THEN [Quantity]
END
```

------------------------------------------------------------------------

## Total PY Sales

**Type:** Continuous Measure

**Purpose:**\
Calculates total sales for the previous year.

**Calculation:**

``` sql
IF YEAR([Order Date]) = { MAX(YEAR([Order Date])) } - 1
THEN [Sales]
END
```

------------------------------------------------------------------------

## YoY Profit

**Type:** Continuous Measure

**Purpose:**\
Calculates year-over-year profit growth percentage.

**Calculation:**

``` sql
(SUM([Total CY Profit]) - SUM([Total PY Profit]))
/ SUM([Total PY Profit])
```

------------------------------------------------------------------------

## YoY Profit Indicator

**Type:** Discrete Measure

**Purpose:**\
Displays directional indicator based on YoY profit trend.

**Calculation:**

``` sql
IF [YOY Profit] > 0 THEN "▲" ELSE "▼" END
```

------------------------------------------------------------------------

## YoY Qty

**Type:** Continuous Measure

**Purpose:**\
Calculates year-over-year quantity growth percentage.

**Calculation:**

``` sql
(SUM([Total CY Qty]) - SUM([Total PY Qty]))
/ SUM([Total PY Qty])
```

------------------------------------------------------------------------

## YoY Qty Indicator

**Type:** Discrete Measure

**Purpose:**\
Displays directional indicator based on YoY quantity trend.

**Calculation:**

``` sql
IF [YOY Qty] > 0 THEN "▲" ELSE "▼" END
```

------------------------------------------------------------------------

## YoY Sales

**Type:** Continuous Measure

**Purpose:**\
Calculates year-over-year sales growth percentage.

**Calculation:**

``` sql
(SUM([Total CY sales]) - SUM([Total PY Sales]))
/ SUM([Total PY Sales])
```

------------------------------------------------------------------------

## YoY Sales Indicator

**Type:** Discrete Measure

**Purpose:**\
Displays directional indicator based on YoY sales trend.

**Calculation:**

``` sql
IF [YOY Sales] > 0 THEN "▲" ELSE "▼" END
```

------------------------------------------------------------------------

## Notes

-   FIXED LOD expressions are used to ensure consistency across filters.
-   Table calculations depend on view context and partitioning.
-   Parameter-driven logic enhances dashboard interactivity.
