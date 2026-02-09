# Maji Ndogo Water Crisis - Project Brief

## Executive Summary

This document outlines the comprehensive analysis of water access challenges in Maji Ndogo, a region where only 34% of the population currently has access to basic water services according to UN standards. The project leverages Power BI to create an interactive dashboard that guides infrastructure improvement efforts across 5 provinces.

## Background

### The Challenge
Maji Ndogo faces a significant water access crisis affecting over 27 million residents across 5 provinces. The national survey revealed critical infrastructure gaps, with the majority of the population lacking reliable access to clean water sources.

### The Opportunity
With strategic infrastructure investments totaling $147M USD, we can improve water access for 64% of the population, bringing basic water services to millions of residents who currently lack this fundamental resource.

## Project Objectives

### Primary Goals
1. **Quantify the Challenge:** Provide clear metrics on current water access levels across all provinces
2. **Identify Solutions:** Catalog specific improvement needs by source type and geographic location
3. **Budget Planning:** Calculate realistic budget requirements with appropriate cost adjustments for rural implementations
4. **Enable Decisions:** Create tools for data-driven decision-making at both national and provincial levels

### Success Metrics
- Clear visualization of current state (34% basic access baseline)
- Projected impact quantification (+64% improvement potential)
- Actionable insights for 5 provincial leaders
- Transparent budget allocation ($147M total across provinces and improvement types)

## Scope

### Geographic Coverage
- **5 Provinces:** Akatsi, Amanzi, Hawassa, Kilmani, Sokoto
- **27+ Million** total population
- **Urban and Rural** communities across all provinces

### Data Sources
- National water survey results
- Infrastructure cost database with rural adjustments
- Population and demographic data
- Water quality testing results (well pollution data)
- Queue time measurements at public water sources

### Analysis Domains
1. **Water Source Assessment**
   - Rivers
   - Wells (clean vs. contaminated)
   - Shared taps (with queue time analysis)
   - Taps in homes
   - Broken infrastructure

2. **Improvement Categories**
   - Drill new wells
   - Install public taps (1-8 tap configurations)
   - Install RO (Reverse Osmosis) filters
   - Install UV and RO filters
   - Repair existing infrastructure

3. **Cost Analysis**
   - Base unit costs by improvement type
   - Rural cost adjustment (+50% premium)
   - Provincial budget allocation
   - Improvement type budget breakdown

## Methodology

### User-Centered Design Approach

The dashboard was built using a user-story driven methodology, ensuring the final product meets real stakeholder needs.

#### User Story 1: National Leadership (President Naledi)
**As President Naledi, I want to:**
1. See key points of survey results to understand overall water access status
2. Know how many people are affected by water access challenges and what those challenges are
3. Understand how much money we need and where it will be spent
4. View data at both national and provincial levels for comprehensive oversight

#### User Story 2: Provincial Leadership
**As a Provincial Leader, I want to:**
1. See province-specific data on population served by each water source type
2. Understand the number of sources by type and whether they're rural or urban
3. Review town-level statistics relevant to my province
4. Access a clear summary of improvements needed and associated costs

### Technical Approach

#### Data Modeling
- Star schema design with fact and dimension tables
- Relationships established between water sources, locations, and costs
- Calculated columns for classifications and aggregations

#### DAX Calculations
- Cost adjustments for rural implementations
- Water access classification based on UN standards
- Queue time aggregations across multiple visits
- Budget summaries at multiple hierarchical levels

#### Visualization Strategy
- Minimal, clean formatting for data clarity
- Consistent color schemes across all reports
- Interactive drill-through capabilities
- Bookmark-based view toggling

## Deliverables

### 1. National Overview Dashboard
- High-impact summary metrics (cards)
- Population distribution by source type (bar charts)
- Provincial comparison visuals
- Improvement quantity analysis
- Budget allocation perspectives (toggle-able views)

### 2. Provincial Report Pages (5 total)
One dedicated page for each province containing:
- Province-specific budget allocation breakdown
- Rural vs. Urban spending distribution
- Town-level data tables
- Improvement quantities by type
- Total provincial budget summary

### 3. Interactive Features
- Province slicer with geographic map
- Drill-through navigation from national to provincial views
- Bookmark-based perspective switching (Province vs. Improvement budget views)
- Dynamic tooltips with contextual information

### 4. Documentation
- Technical documentation of DAX formulas
- User guide for dashboard navigation
- Data dictionary for field definitions
- Methodology documentation

## Timeline & Phases

This analysis represents **Part 3** of an integrated project:

### Part 1: Data Collection & Survey Design
- Survey instrument development
- Field data collection
- Initial data validation

### Part 2: Data Cleaning & Transformation
- Data quality assessment
- ETL processes
- Data model creation
- Relationship establishment

### Part 3: Visualization & Reporting (Current Phase)
- Dashboard design and development
- DAX calculation implementation
- User testing and iteration
- Final documentation

## Key Stakeholders

### Primary Users
- **President Aziza Naledi** - National strategic decision-making
- **Provincial Leaders (5)** - Local implementation and resource allocation
- **Project Management Team** - Execution oversight and monitoring

### Supporting Stakeholders
- **Finance Department** - Budget approval and fund disbursement
- **Infrastructure Teams** - Technical implementation
- **Community Representatives** - Local input and feedback

## Budget Overview

### Total Investment Required: $147,737,375 USD

#### By Province:
- Akatsi: $31,355,075 (21.37%)
- Amanzi: $13,428,275 (9.15%)
- Hawassa: $22,553,325 (15.37%)
- Kilmani: $39,249,225 (26.75%)
- Sokoto: $40,151,475 (27.36%)

#### By Improvement Type:
- Drill Well: 3,400 improvements
- Install Public Tap(s): 3,700 improvements
- Install RO Filter: 7,100 improvements
- Install UV and RO Filter: 5,400 improvements
- Repair Infrastructure: 5,900 improvements

## Expected Impact

### Quantitative Outcomes
- **Population Reach:** 27+ million people
- **Access Improvement:** +64 percentage points (from 34% to 98%)
- **Infrastructure Additions:** 25,398 improvements
- **Geographic Coverage:** All 5 provinces served

### Qualitative Benefits
- Improved public health outcomes
- Reduced time burden (especially for women and children)
- Enhanced economic productivity
- Increased school attendance
- Better quality of life

## Risk Considerations

### Data Quality
- Survey accuracy and completeness
- Cost estimate reliability
- Population data currency

### Implementation Challenges
- Rural accessibility and logistics
- Local capacity for maintenance
- Supply chain constraints
- Political and social factors

### Mitigation Strategies
- Phased implementation approach
- Local community engagement
- Ongoing monitoring and evaluation
- Adaptive management processes

## Water Access Standards (UN Reference)

The project uses internationally recognized water access standards:

| Level | Criteria | Classification |
|-------|----------|----------------|
| Safely Managed | On premises, available when needed, free from contamination | Best |
| **Basic** | **≤30 min collection time including queuing** | **Target** |
| Limited | >30 min collection time | Below Target |
| Unimproved | Unprotected well or spring | Needs Improvement |
| Surface Water | Direct from river/lake | Critical Need |

**Project Goal:** Move all residents to at least "Basic" level access.

## Next Steps

1. **Validation:** Stakeholder review and feedback on dashboard
2. **Refinement:** Incorporate feedback and optimize visualizations
3. **Deployment:** Publish to Power BI Service for stakeholder access
4. **Training:** Conduct user training sessions for provincial leaders
5. **Monitoring:** Establish baseline and track progress over time

## Contact & Project Information

**Project Lead:** Chris Frost  
**Educational Partner:** ALX Africa  
**Project Type:** Data Analytics Capstone  
**Tools Used:** Power BI Desktop, DAX, Power Query

---

*This project brief is part of the educational curriculum provided by ALX Africa. The scenario and data are designed to simulate real-world water access challenges while building practical data analysis skills.*
