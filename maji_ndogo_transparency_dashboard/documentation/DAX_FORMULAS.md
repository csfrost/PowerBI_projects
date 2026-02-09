# DAX Formulas Reference
## Maji Ndogo Transparency Dashboard

This document provides a complete reference of all DAX measures and calculated columns used in the dashboard.

---

## Table of Contents
1. [Project Progress Metrics](#project-progress-metrics)
2. [Budget & Cost Tracking](#budget--cost-tracking)
3. [Population Impact Measures](#population-impact-measures)
4. [Utility Measures](#utility-measures)
5. [Calculated Columns](#calculated-columns)

---

## Project Progress Metrics

### total_improvements
Calculates the total number of water source improvements for the selected town/area.

```dax
total_improvements = 
CALCULATE(
    COUNTROWS('project_progress'),
    ALLEXCEPT('project_progress', 'project_progress'[town])
)
```

**Purpose**: Denominator for completion percentage calculations  
**Returns**: Integer count of all improvements  
**Filter Context**: Respects town filter only  

---

### number_completed_projects
Counts projects with "Complete" status.

```dax
number_completed_projects = 
CALCULATE(
    COUNTROWS('project_progress'),
    'project_progress'[source_status] = "Complete"
)
```

**Purpose**: Numerator for completion percentage  
**Returns**: Integer count of completed projects  
**Filter Context**: Respects all filters  

---

### pct_project_complete
Calculates percentage of completed projects.

```dax
pct_project_complete = 
DIVIDE(
    [number_completed_projects],
    [total_improvements],
    0
)
```

**Purpose**: Main KPI for project progress  
**Returns**: Decimal (format as percentage)  
**Error Handling**: Returns 0 if division by zero  

---

### sources_remaining
Calculates how many sources still need improvement.

```dax
sources_remaining = 
[total_improvements] - [number_completed_projects]
```

**Purpose**: Shows backlog of work  
**Returns**: Integer  
**Display**: "25,369 sources to go"  

---

## Budget & Cost Tracking

### cumulative_budget
Running total of budgeted costs up to the selected date.

```dax
cumulative_budget = 
CALCULATE(
    SUM('project_progress'[budgeted_improvement_cost]),
    FILTER(
        ALL('project_progress'[date_of_completion]),
        'project_progress'[date_of_completion] <= MAX('project_progress'[date_of_completion]) &&
        NOT(ISBLANK('project_progress'[date_of_completion]))
    )
)
```

**Purpose**: Target line for KPI chart  
**Returns**: Currency  
**Time Intelligence**: Cumulative calculation up to max date  
**Null Handling**: Excludes blank completion dates  

---

### cumulative_cost
Running total of actual costs incurred up to the selected date.

```dax
cumulative_cost = 
CALCULATE(
    SUM('project_progress'[cost]),
    FILTER(
        ALL('project_progress'[date_of_completion]),
        'project_progress'[date_of_completion] <= MAX('project_progress'[date_of_completion]) &&
        NOT(ISBLANK('project_progress'[date_of_completion]))
    )
)
```

**Purpose**: Actual spending line for KPI chart  
**Returns**: Currency  
**Comparison**: Compare against cumulative_budget to see variance  

---

### budget_variance
Difference between actual and budgeted costs.

```dax
budget_variance = 
[cumulative_cost] - [cumulative_budget]
```

**Purpose**: Shows over/under budget amount  
**Returns**: Currency (positive = over budget)  
**Formatting**: Conditional formatting based on positive/negative  

---

### budget_variance_pct
Percentage variance from budget.

```dax
budget_variance_pct = 
DIVIDE(
    [budget_variance],
    [cumulative_budget],
    0
)
```

**Purpose**: Standardized variance metric  
**Returns**: Percentage  
**Example**: 0.10 = 10% over budget  

---

## Population Impact Measures

### total_population
Total population served by water sources in the filtered area.

```dax
total_population = 
CALCULATE(
    SUM('water_source'[number_of_people_served]),
    ALLEXCEPT('project_progress', 'project_progress'[town])
)
```

**Purpose**: Denominator for access percentage calculations  
**Returns**: Integer  
**Filter Context**: Respects town filter only  

---

### population_with_basic_access
Population with basic water access before improvements.

```dax
population_with_basic_access = 
CALCULATE(
    SUM('water_source'[number_of_people_served]),
    FILTER(
        ALL(water_source),
        OR(
            OR(
                AND(
                    'water_source'[type_of_water_source] = "well",
                    RELATED(well_pollution[results]) = "Clean"
                ),
                'water_source'[type_of_water_source] = "tap_in_home"
            ),
            AND(
                'water_source'[type_of_water_source] = "shared_tap",
                'water_source'[Average_queue_time] < 30
            )
        )
    )
)
```

**Purpose**: Baseline water access before project  
**Returns**: Integer population count  
**Logic**: 
- Clean wells OR
- Tap in home OR
- Shared tap with <30 min queue

---

### population_now_basic_access
Population gaining access through completed improvements.

```dax
population_now_basic_access = 
CALCULATE(
    SUM('water_source'[number_of_people_served]),
    FILTER(
        ALL(water_source),
        AND(
            'water_source'[type_of_water_source] = "river",
            RELATED('project_progress'[source_status]) = "Complete"
        )
    )
) + 
CALCULATE(
    SUM('water_source'[number_of_people_served]),
    FILTER(
        ALL(water_source),
        AND(
            'water_source'[type_of_water_source] = "well",
            RELATED(well_pollution[results]) <> "Clean",
            RELATED('project_progress'[source_status]) = "Complete"
        )
    )
) +
CALCULATE(
    SUM('water_source'[number_of_people_served]),
    FILTER(
        ALL(water_source),
        AND(
            'water_source'[type_of_water_source] = "shared_tap",
            'water_source'[Average_queue_time] >= 30,
            RELATED('project_progress'[source_status]) = "Complete"
        )
    )
)
```

**Purpose**: New people with access from improvements  
**Returns**: Integer  
**Logic**: Rivers upgraded to wells + Contaminated wells cleaned + Long-queue taps improved  

---

### pct_population_now_basic_access
Percentage of population with basic access including improvements.

```dax
pct_population_now_basic_access = 
DIVIDE(
    [population_with_basic_access] + [population_now_basic_access],
    [total_population],
    0
)
```

**Purpose**: Main impact KPI  
**Returns**: Percentage  
**Trend**: Started at 34%, ended at 77%  

---

## Utility Measures

### avg_cost_per_improvement
Average cost across all completed improvements.

```dax
avg_cost_per_improvement = 
DIVIDE(
    SUM('project_progress'[cost]),
    COUNTROWS('project_progress'),
    0
)
```

**Purpose**: Vendor comparison metric  
**Returns**: Currency  
**Note**: Must control for location and improvement type  

---

### completed_project_count
Simple count of completed projects (for tables).

```dax
completed_project_count = 
COUNTROWS('project_progress')
```

**Purpose**: Display count in vendor tables  
**Returns**: Integer  
**Filter Context**: Automatically filtered by visuals  

---

## Calculated Columns

### budgeted_improvement_cost
Estimated cost per improvement accounting for location and type.

```dax
budgeted_improvement_cost = 
IF(
    CONTAINSSTRING('project_progress'[town], "Rural"),
    RELATED('infrastructure_cost'[Rural_adjusted_cost]),
    RELATED('infrastructure_cost'[unit_cost_USD])
)
```

**Purpose**: Budget baseline for each project  
**Column Type**: Calculated column in project_progress table  
**Logic**: Rural areas use adjusted (higher) cost  
**Note**: Uses CONTAINSSTRING to match "Rural_Sokoto", "Rural_Akatsi", etc.  

---

### project_duration
Days from start to completion.

```dax
project_duration = 
DATEDIFF(
    'project_progress'[date_started],
    'project_progress'[date_of_completion],
    DAY
)
```

**Purpose**: Analyze relationship between duration and cost  
**Column Type**: Calculated column  
**Returns**: Integer (days)  
**Null Handling**: Returns BLANK() if either date is missing  

---

### cost_per_citizen
Cost divided by population served.

```dax
cost_per_citizen = 
DIVIDE(
    'project_progress'[cost],
    RELATED('water_source'[number_of_people_served]),
    0
)
```

**Purpose**: Standardized cost metric  
**Column Type**: Calculated column  
**Returns**: Currency  
**Usage**: Compare efficiency across different sized projects  

---

## DAX Best Practices Used

### 1. DIVIDE() for Safe Division
Always use `DIVIDE(numerator, denominator, alternate_result)` instead of `/` operator to handle division by zero gracefully.

### 2. ALLEXCEPT() for Selective Filtering
Use when you want to respect only specific filters:
```dax
ALLEXCEPT('table', 'table'[column_to_keep])
```

### 3. RELATED() for Dimension Lookups
Access related table columns in calculated columns:
```dax
RELATED('dimension_table'[column])
```

### 4. CALCULATE() with FILTER() for Complex Conditions
Combine for precise control over filter context:
```dax
CALCULATE(
    expression,
    FILTER(table, condition)
)
```

### 5. NOT(ISBLANK()) for Null Handling
Check for non-blank values in date fields:
```dax
NOT(ISBLANK('table'[date_column]))
```

### 6. Meaningful Measure Names
Use descriptive names that indicate what is calculated:
- ✅ `pct_project_complete`
- ❌ `measure1`

### 7. Comment Complex Logic
While DAX doesn't support inline comments, document formulas in separate files (like this one).

---

## Performance Optimization Notes

### Calculated Columns vs Measures
- **Columns**: Computed at refresh time, stored in model (use for frequently filtered fields)
- **Measures**: Computed at query time (use for aggregations and dynamic calculations)

### This Dashboard's Choices
- Used calculated columns for: `budgeted_improvement_cost`, `project_duration`
- Used measures for: All aggregations, percentages, cumulative calculations

### Why?
- Budget cost needs to be sliced/diced → Column
- Cumulative totals change with date filter → Measure
- Population percentages depend on filter context → Measure

---

## Testing Checklist

When modifying DAX measures, verify:

- [ ] Measure returns expected value at national level
- [ ] Measure respects province filter correctly
- [ ] Measure respects town filter correctly
- [ ] Measure respects date slicer correctly
- [ ] Measure handles null/blank values appropriately
- [ ] Measure performance is acceptable (<2 seconds)
- [ ] Measure formatting is correct (%, currency, etc.)

---

**Last Updated**: 2024  
**Dashboard Version**: 1.0  
**Power BI Version**: Desktop (latest)
