# Maji Ndogo Water Infrastructure Transparency Dashboard

[![Power BI](https://img.shields.io/badge/Power%20BI-Desktop-yellow?logo=powerbi)](https://powerbi.microsoft.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![ALX Africa](https://img.shields.io/badge/ALX-Africa-green)](https://www.alxafrica.com/)

An interactive Power BI dashboard tracking the progress, costs, and impact of a multi-year water infrastructure improvement project in Maji Ndogo. This comprehensive analytics solution monitors 25,369 water sources across five provinces, providing real-time transparency to stakeholders and enabling data-driven decision-making for budget optimization.

![](visualizations/national.png)

## Table of Contents

- [Background](#background)
- [Key Features](#key-features)
- [Dashboard Components](#dashboard-components)
- [Technical Implementation](#technical-implementation)
- [Key Insights & Findings](#key-insights--findings)
- [Project Structure](#project-structure)
- [Skills Demonstrated](#skills-demonstrated)
- [Installation & Usage](#installation--usage)
- [Visualizations](#visualizations)
- [Related Projects](#related-projects)
- [Acknowledgments](#acknowledgments)
- [Contact](#contact)

## Background

This project is the fourth phase of the Maji Ndogo water crisis analysis, focusing on tracking the implementation of water infrastructure improvements across the region. The dashboard serves dual purposes:

### Public Transparency
Allowing citizens to see where funds are being allocated and what improvements are being made in their communities.

### Project Management
Enabling decision-makers to monitor progress, control costs, and optimize vendor performance.

**Project Timeline**: January 2023 - December 2027  
**Total Budget**: $146.7M (initial allocation)  
**Improvements Tracked**: 25,369 water sources  
**Population Impacted**: 28M citizens  
**Provinces Covered**: 5 (Sokoto, Akatsi, Amanzi, Hawassa, Kilimani)

## Key Features

### Real-Time Progress Tracking
- Project completion percentage with time-series visualization
- Water source improvement status by location
- Population access to basic water services metrics
- Interactive map showing completion rates by province and town

### Budget & Cost Analysis
- Cumulative cost vs. budget KPI tracking
- Budget variance analysis by province and improvement type
- Cost breakdown by water source type (wells, taps, filters, infrastructure)
- Rural vs. urban cost comparisons

### endor Performance Analytics
- Vendor efficiency analysis controlling for location and job type
- Cost optimization recommendations
- Travel pattern analysis revealing cost drivers
- Comparative vendor performance metrics

### Interactive Filtering
- Date range sliders for temporal analysis
- Province and town selectors
- Improvement type filters
- Rural/urban classification toggles

## Dashboard Components

### 1. National Overview Page
- Current project status (100% completion)
- Population with basic water access (34% → 77%)
- Total budget utilization tracking
- Interactive map visualization with province drill-down
- Key metrics cards showing impact

### 2. Cost Analysis & Vendor Performance Page
- Budget vs. actual cost KPI chart with trend indicators
- Cost breakdown by improvement type (pie chart)
- Cost breakdown by source type (bar chart)
- Vendor performance comparison table
- Dynamic filtering by location and improvement type

### 3. Key Influencers Analysis Page
- AI-powered analysis of factors driving cost variations
- Rural vs. urban cost disparities (2x difference identified)
- Provincial cost analysis
- Project duration impact on costs

### 4. Province Drill-Through Pages
Individual detailed views for each of the five provinces:
- **Sokoto**: $40.2M budget, 5,603 improvements, $6.95/citizen
- **Akatsi**: $31.4M budget, 4,963 improvements, $5.23/citizen
- **Amanzi**: $13.4M budget, 3,748 improvements, $2.47/citizen
- **Hawassa**: $22.6M budget, 4,384 improvements, $5.87/citizen
- **Kilimani**: $39.2M budget, 6,700 improvements, $5.96/citizen

Each showing:
- Budget allocation breakdown
- Rural/urban spending distribution
- Improvement type quantities and costs
- Town-level budget summaries
- Cost per citizen calculations

## Technical Implementation

### Data Model Architecture
- **Star Schema Design**: Central fact table (project_progress) with related dimension tables
- **Dimension Tables**: location, water_source, infrastructure_cost, vendors, well_pollution
- **Optimized Relationships**: One-to-many with proper bi-directional filtering where needed
- **Calculated Columns**: Rural cost adjustments, improvement classifications, aggregated types

### Advanced DAX Measures

**Project Progress Metrics**:
```dax
total_improvements = 
    CALCULATE(
        COUNTROWS('project_progress'),
        ALLEXCEPT('project_progress', 'project_progress'[town])
    )

number_completed_projects = 
    CALCULATE(
        COUNTROWS('project_progress'),
        'project_progress'[source_status] = "Complete"
    )

pct_project_complete = 
    DIVIDE([number_completed_projects], [total_improvements], 0)
```

**Budget Tracking with Time Intelligence**:
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

**Population Impact Analysis**:
```dax
total_population = 
    CALCULATE(
        SUM('water_source'[number_of_people_served]),
        ALLEXCEPT('project_progress', 'project_progress'[town])
    )

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

pct_population_now_basic_access = 
    DIVIDE(
        [population_with_basic_access] + [population_now_basic_access],
        [total_population],
        0
    )
```

### Data Transformations & Optimization
- Removed unnecessary columns to improve performance
- Standardized date formatting (YYYY/MM/DD)
- Implemented dynamic town naming with province suffixes
- Created budget calculation formulas accounting for rural premiums
- Applied CONTAINSSTRING() for flexible location matching

### Visualization Techniques
- **Custom Map Visual**: JSON shape map for accurate regional representation
- **KPI Charts**: Goal-based tracking with status indicators
- **Key Influencers**: AI-powered cost driver analysis
- **Custom Tooltips**: Rich hover information with contextual data
- **Conditional Formatting**: Color-coded metrics for quick insights

## Key Insights & Findings

### 1. Budget Performance
- **Initial Projection**: 10% over budget after year 1
- **Final Status**: Achieved 100% completion within acceptable variance
- **Sokoto Challenge**: 40% budget overrun in most difficult province

### 2. Cost Drivers Identified
- **Rural vs Urban**: Rural improvements cost 2x urban (accessibility challenges)
- **Travel Impact**: Vendor travel time significantly affects project costs
- **Provincial Variation**: Sokoto $8.04/citizen vs. Amanzi $2.47/citizen

### 3. Vendor Performance Discoveries

**Case Study: Entebbe RO Installers (ERI893)**
- Initially appeared most expensive ($4,009 avg cost)
- Controlled analysis revealed highest efficiency
- Completed most projects (703) through strategic job selection
- Minimized travel costs by working in concentrated areas

**Comparison: Ouagadougou Waterworks (OW290)**
- Lower unit costs on surface ($2,821 avg)
- Higher total project costs due to inefficient routing
- Excessive travel between distant job sites
- Completed fewer projects (312)

### 4. Optimization Recommendations
✅ Created vendor training materials on optimal job selection  
✅ Encouraged proximity-based assignment strategies  
✅ Implemented routing optimization guidelines  
✅ Resulted in improved cost control in phases 2-4  

### 5. Impact Metrics
- **Water Access Improvement**: 34% → 77% population with basic access
- **Project Completion**: 100% of 25,369 sources addressed
- **Early Impact**: 11,000+ people helped in first phase
- **Budget Efficiency**: Stayed within 5% of revised projections

## Project Structure

```
maji_ndogo_transparency_dashboard/
│
├── README.md                          # This file
├── LICENSE                            # MIT License
├── .gitignore                        # Git ignore patterns
│
├── visualizations/                   # Visual documentation
│   ├── national_overview.png
│   ├── sokoto_report.png
│   ├── akatsi_report.png
│   ├── amanzi_report.png
│   ├── hawassa_report.png
│   └── kilimani_report.png
│
├── documentation/                    # Additional documentation
    ├── DATA_DICTIONARY.md           # Data structure reference
    ├── DAX_FORMULAS.md              # Complete DAX reference
```

**Note**: Dataset files (`Md_water_services_data.xlsx`) and the Power BI file (`P4.pbix`) are proprietary to ALX Africa and are not included in this public repository.

## Skills Demonstrated

### Data Analysis & Visualization
✓ Complex DAX measure creation for KPIs and time-intelligence  
✓ Star schema data modeling best practices  
✓ Interactive dashboard design following UX principles  
✓ Geospatial data visualization  
✓ Performance optimization techniques  

### Business Intelligence
✓ Stakeholder requirement gathering and translation  
✓ Budget variance analysis and forecasting  
✓ Vendor performance evaluation frameworks  
✓ Cost driver identification using AI visuals  
✓ Multi-level drill-through navigation  

### Problem Solving
✓ Context-aware metric calculation (town filtering with date independence)  
✓ Handling null values in complex date comparisons  
✓ Creating fair vendor comparisons with confounding variables  
✓ Balancing public transparency with management insights  

### Communication
✓ Dual-audience dashboard design (citizens vs. decision-makers)  
✓ Data storytelling through progressive insight revelation  
✓ Actionable recommendation generation from analytics  
✓ Clear documentation and knowledge transfer  

## Installation & Usage

### Prerequisites
- **Power BI Desktop** (latest version recommended)
- **Windows OS** (for Power BI Desktop)
- Access to dataset (contact ALX Africa for educational access)

### Setup Instructions

1. **Clone the Repository**
   ```bash
   git clone https://github.com/csfrost/PowerBI_projects.git
   cd PowerBI_projects/maji_ndogo_transparency_dashboard
   ```

2. **Download Power BI File**
   - The `.pbix` file is not included due to data privacy
   - Contact repository owner or ALX Africa for access

3. **Configure Data Sources** (if you have access to data)
   - Open Power BI Desktop
   - File → Options and settings → Data source settings
   - Update paths to point to your local `Md_water_services_data.xlsx`
   - Update path to `MD_Full_map.json` for map visual

4. **Refresh Data**
   - Click "Refresh" to load the data model
   - Verify all visuals render correctly

### Navigation Guide

**Main Dashboard**:
- Use the **date range slicer** to filter by time period
- Click on **provinces in the map** to filter all visuals
- Observe the **KPI card** color changes (red = over budget, green = under budget)

**Province Deep Dive**:
- Right-click any province name in tables or charts
- Select **"Drill through"** → Choose specific province page
- Use the back button to return to main dashboard

**Vendor Analysis**:
- Navigate to vendor analysis page via page tabs
- Use **improvement type slicer** to filter by job category
- Toggle **rural/urban filter** to compare like-for-like
- Click on vendor names to see their geographic coverage on map

**Tips for Best Experience**:
- Start with national view to understand overall progress
- Use date slicer to see project evolution over time
- Compare provinces to identify patterns
- Drill into specific vendors when analyzing cost efficiency

## Visualizations

### National Overview Dashboard
![](visualizations/national.png)
*Main dashboard showing project completion, population access improvement (34%→77%), budget tracking KPI, and interactive map with provincial completion rates*

### Sokoto Province Report
![](visualizations/sokoto.png)
*Detailed analysis of Sokoto showing $40.2M budget allocation, 49.3% rural vs 8.8% urban spending split, and breakdown of 5,603 improvements including 1,709 wells drilled*

### Akatsi Province Report
![](visualizations/akatsi.png)
*Akatsi province overview with $31.4M budget, 4,963 improvements, showing lower cost per citizen ($5.23) and different improvement mix with focus on RO filter installations (2,056)*

### Amanzi Province Report
![](visualizations/amanzi.png)
*Amanzi province showing $13.4M budget, 3,748 improvements, and the lowest cost per citizen ($2.47) demonstrating efficient resource utilization in more accessible areas*

### Hawassa Province Report
![](visualizations/hawassa.png)
*Hawassa province details with $22.6M budget, 4,384 improvements, balanced rural/urban distribution, and $5.87 cost per citizen*

### Kilimani Province Report
![](visualizations/kilimani.png)
*Kilimani province showing largest scope with $39.2M budget, 6,700 improvements across diverse terrain, $5.96 cost per citizen*

## Related Projects

This project is part of a comprehensive **Maji Ndogo Water Analysis** series:

1. **[Crisis Analysis Dashboard](https://github.com/csfrost/PowerBI_projects/tree/main/crisis_analysis_maji_ndogo)** - Initial assessment identifying 25,369 water sources needing improvement
2. **SQL Projects** - Cleaning and exploration of 60,000+ survey records
    - **[Audit Investigation (SQL)](https://github.com/csfrost/SQL_projects/tree/main/audit_investigation)**
    - **[Audits to Action (SQL)](https://github.com/csfrost/SQL_projects/tree/main/audits_to_actions)**
    - **[Clustering Water Crisis (SQL)](https://github.com/csfrost/SQL_projects/tree/main/clustering_water_crisis)**
4. **Transparency Dashboard** *(This Project)* - Implementation tracking and performance optimization

## Acknowledgments

This project was completed as part of the **ALX Africa Data Analytics Program** integrated project series.

**Special Thanks**:
- **[ALX Africa](https://www.alxafrica.com/)** for providing the comprehensive dataset, project structure, and learning framework that made this analysis possible
- **ExploreAI Academy** for curriculum development, technical guidance, and the immersive narrative approach
- **Maji Ndogo Fictional Team** for the engaging storyline that contextualized complex data analytics concepts

**Data Source**: ALX Africa - Maji Ndogo Water Services Dataset (Proprietary)

**Educational Context**: This project uses a fictional dataset created for educational purposes. While the data is simulated, the analytical techniques and business intelligence methods applied are industry-standard and transferable to real-world scenarios.

---

## Contact

**Author**: [csfrost](https://github.com/csfrost)  

**LinkedIn**: [Connect with me on LinkedIn](#)  
**Portfolio**: [View my portfolio](#)  

For questions about this project or collaboration opportunities:
- Open an [Issue](https://github.com/csfrost/PowerBI_projects/issues)
- Submit a [Pull Request](https://github.com/csfrost/PowerBI_projects/pulls)
- Reach out via LinkedIn

---

## License

**Code & Documentation**: MIT License - see [LICENSE](LICENSE) file for details

**Dataset Rights**: Reserved by ALX Africa. Dataset not included in this repository.

**Power BI File**: Not distributed publicly due to data licensing restrictions

---

<div align="center">

**⭐ If you found this project helpful, please consider giving it a star!**

Made with ❤️ and Power BI by [csfrost](https://github.com/csfrost)

</div>
