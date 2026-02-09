# Maji Ndogo Water Crisis Analysis - Power BI Dashboard

A comprehensive Power BI dashboard analyzing water access challenges across Maji Ndogo and providing data-driven insights for infrastructure improvement planning.

## Project Overview

This project visualizes the results of a national water survey in Maji Ndogo, helping decision-makers understand:
- Current water access levels across 5 provinces
- Infrastructure improvement requirements
- Budget allocation for water source upgrades
- Impact metrics for planned interventions

**Key Findings:**
- **Current Basic Water Access:** 34% of population
- **Total Budget Required:** $147M USD
- **Projected Improvement:** +64% population with basic water access
- **Total Improvements Needed:** 25,398 across all provinces

## Geographic Coverage

The analysis covers 5 provinces in Maji Ndogo:
- **Akatsi** - 5.99M population | 4,963 improvements | $31.4M budget
- **Amanzi** - 5.43M population | 3,748 improvements | $13.4M budget
- **Hawassa** - 3.84M population | 4,384 improvements | $22.6M budget
- **Kilmani** - 6.58M population | 6,700 improvements | $39.2M budget
- **Sokoto** - 5.77M population | 5,603 improvements | $40.2M budget

## Dashboard Features

### National Overview
- **High-Impact Metrics:** Total budget, current access %, improvement percentage
- **Population Analysis:** Rural vs. Urban water source distribution
- **Improvement Breakdown:** Quantity and type of infrastructure upgrades needed
- **Provincial Comparison:** Budget allocation and improvement distribution
- **Interactive Filters:** Province selector with geographic visualization

![](docs/visualizations/national.png)
*Interactive national dashboard with provincial filters and drill-through capabilities*

### Provincial Deep-Dive Reports
Each province has a dedicated report page showing:
- Budget allocation by improvement type (donut chart)
- Rural/Urban spending distribution (pie chart)
- Town-level breakdown (data table)
- Improvement quantities by type (bar chart)
- Total provincial budget card

#### Sample Provincial Reports

**Sokoto Province Report**
![Sokoto Report](docs/visualizations/sokoto.png)

**Akatsi Province Report**
![](docs/visualizations/akatsi.png)

**Amanzi Province Report**
![](docs/visualizations/amanzi.png)

**Hawassa Province Report**
![](docs/visualizations/Hawassa.png)

**Kilmani Province Report**
![](docs/visualizations/kilmani.png)

### Improvement Categories
1. **Drill Well** - 3.4K improvements
2. **Install Public Tap(s)*** - 3.7K improvements
3. **Install RO Filter** - 7.1K improvements
4. **Install UV and RO Filter** - 5.4K improvements
5. **Repair Infrastructure** - 5.9K improvements

*Aggregated category combining Install 1-8 taps

## Technical Implementation

### DAX Calculations

**Rural Cost Adjustment:**
```dax
Rural_adjusted_cost = 
    infrastructure_cost[unit_cost_USD] * 1.5
```

**Budgeted Improvement Cost:**
```dax
Budgeted_improvement_cost =
IF(
    'project_progress'[location_type] == "Rural",
    RELATED('infrastructure_cost'[Rural_adjusted_cost]),
    RELATED('infrastructure_cost'[unit_cost_USD])
)
```

**Average Queue Time:**
```dax
Average_queue_time =
CALCULATE(
    AVERAGE('visits'[time_in_queue]),
    FILTER(
        'visits',
        'visits'[source_id] = 'water_source'[source_id]
    )
)
```

**Basic Water Access Classification:**
```dax
Basic_water_access =
IF(
    AND(
        'water_source'[type_of_water_source] = "well",
        RELATED('well_pollution'[results]) = "Clean"
    ),
    "Basic Access",
    IF(
        'water_source'[type_of_water_source] = "tap_in_home",
        "Basic Access",
        IF(
            AND(
                'water_source'[type_of_water_source] = "shared_tap",
                'water_source'[Average_queue_time] < 30
            ),
            "Basic Access",
            "Below Basic Access"
        )
    )
)
```

**Improvement Type Aggregation:**
```dax
Aggregated_improvements =
IF(
    CONTAINSSTRING(
        'project_progress'[improvement], "Install"
    ),
    "Install public tap(s)*",
    IF(
        'project_progress'[improvement] == "Diagnose local infrastructure",
        "Repair infrastructure",
        'project_progress'[improvement]
    )
)
```

### Data Model
- **Fact Table:** project_progress (improvement records)
- **Dimension Tables:** 
  - water_source (source details and classification)
  - infrastructure_cost (unit costs and rural adjustments)
  - location (province and town information)
  - well_pollution (water quality data)
  - visits (queue time and survey data)

### Interactive Features
- **Bookmarks:** Toggle between Province and Improvement budget views
- **Drill-Through:** Click provinces on national view to access detailed provincial reports
- **Slicers:** Province selection with map visualization
- **Custom Buttons:** Toggle between different data perspectives

## Water Access Classification (UN Standards)

| Service Level | Definition |
|---------------|------------|
| **Safely Managed** | Accessible on premises, available when needed, free from contamination |
| **Basic** | Collection time ≤ 30 minutes for round trip including queuing |
| **Limited** | Collection time > 30 minutes for round trip including queuing |
| **Unimproved** | Unprotected dug well or unprotected spring |
| **Surface Water** | Directly from river, dam, lake, pond, stream, canal, or irrigation canal |

## Visualization Guidelines

- **Minimal Formatting:** Clean design focused on data clarity
- **Consistent Color Scheme:** Professional color palette across all visuals
- **User-Centric Design:** Built from user stories to meet decision-maker needs
- **Data Hierarchy:** Clear information architecture from national to provincial to town level

## Dashboard Gallery

<table>
  <tr>
    <td width="50%">
      <img src="docs/visualizations/national.png" alt="National Overview" />
      <p align="center"><i>National Overview Dashboard</i></p>
    </td>
    <td width="50%">
      <img src="docs/visualizations/sokoto.png" alt="Sokoto Province" />
      <p align="center"><i>Sokoto Provincial Report</i></p>
    </td>
  </tr>
  <tr>
    <td width="50%">
      <img src="docs/visualizations/akatsi.png" alt="Akatsi Province" />
      <p align="center"><i>Akatsi Provincial Report</i></p>
    </td>
    <td width="50%">
      <img src="docs/visualizations/kilmani.png" alt="Kilmani Province" />
      <p align="center"><i>Kilmani Provincial Report</i></p>
    </td>
  </tr>
</table>

## Repository Structure

```
crisis_analysis_maji_ndogo/
├── README.md
├── LICENSE
├── .gitignore
├── CONTRIBUTING.md
├── docs/
│   ├── project-brief.md
│   └── visualizations/
│       ├── national.png
│       ├── sokoto.png
│       ├── akatsi.png
│       ├── amanzi.png
│       ├── hawassa.png
│       └── kilmani.png
└── reports/
    └── P3.pbix
```

## Usage

1. Download or clone this repository
2. Open `reports/P3.pbix` in Power BI Desktop
3. Use the province slicer on the National page to filter data
4. Click "Province" or "Improvements" buttons to toggle budget view perspectives
5. Right-click any province in charts to drill through to provincial reports
6. Use the back button to return to the National overview

## Methodology

This project follows a user-story driven approach:

**President Naledi's Requirements:**
1. See key survey results for overall water access status
2. Understand how many people are affected and their challenges
3. Know total budget needed and spending breakdown
4. View data at both national and provincial levels

**Provincial Leaders' Requirements:**
1. Province-specific population served by water source type
2. Number of sources by type and location (rural/urban)
3. Town-level statistics and relevant provincial data
4. Summary of improvements and associated costs

## Impact

This dashboard enables:
- **Strategic Planning:** Evidence-based budget allocation across provinces
- **Accountability:** Clear tracking of improvement types and costs
- **Progress Monitoring:** Baseline measurement (34% access) for future impact assessment
- **Localized Decision-Making:** Provincial leaders can prioritize town-level interventions

## Acknowledgments

This project was developed as part of the Data Analytics program with **ALX Africa**. The comprehensive dataset and project structure were provided by ALX Africa as part of their curriculum in building real-world data analysis skills.

Special thanks to:
- **ALX Africa** for the educational opportunity and structured learning path
- The instructional team for guidance on Power BI best practices and data visualization principles
- Fellow learners for collaborative problem-solving throughout the program

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

**Note:** The dataset used in this project is proprietary to ALX Africa and cannot be redistributed. This repository contains only the Power BI report file and documentation.

## Author

**CSFrost**
- GitHub: [@csfrost](https://github.com/csfrost)
- Portfolio: [PowerBI Projects](https://github.com/csfrost/PowerBI_projects)

## Contact

For questions about this project or collaboration opportunities, please open an issue in this repository or reach out via GitHub.

---

*This is a portfolio project created for educational purposes as part of ALX Africa's Data Analytics curriculum. The data and scenario are fictional but based on real-world water access challenges facing communities globally.*
