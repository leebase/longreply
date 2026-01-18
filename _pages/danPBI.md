---
layout: default
title: "Dan Learns PowerBI"
permalink: /danpbi/
---  
This curriculum is designed to transition an Operations Leader from technical zero to a Power BI practitioner using a Six Sigma **DMAIC** (Define, Measure, Analyze, Improve, Control) framework.

# DanData: Power BI Mastery for Ops Leaders

**Audience:** Dan (Six Sigma Black Belt)  
**Goal:** Master Power BI through a DMAIC workflow with verification gates.  
**Repo:** [leebase/dan-data](https://github.com/leebase/dan-data.git)

---

## Module 0: Install & Setup

*Estimated Time: 1-2 Hours*

### 0.1 Power BI Desktop

Power BI Desktop is your authoring environment. Learn the basics of installation and interface navigation.

* **Action:** Install via [Microsoft Store](https://aka.ms/pbidesktopstore).  
* **Resource:** [Getting Started with Desktop](https://learn.microsoft.com/en-us/power-bi/fundamentals/desktop-getting-started)  
* **Additional Learning:** [Power BI Desktop Overview Tutorial](https://learn.microsoft.com/en-us/power-bi/fundamentals/desktop-what-is-desktop) – Covers the UI and basic features.

### 0.2 Git & Repository

Professional analytics requires version control. Start with Git basics if you're new.

* **Action:** Install Git and clone the project.
```bash
# Verify installation
git version

# Clone the project
cd ~
git clone https://github.com/leebase/dan-data.git
cd dan-data
```

* **Definition of Done:** You see workspace/mental_health_demo.sqlite in your folder.  
* **Additional Learning:** [Git Installation Guide](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) and [Cloning a Repository Tutorial](https://docs.github.com/en/repositories/creating-and-managing-repositories/cloning-a-repository) – Essential for version control in data projects.

### 0.3 Verification Web App

This provides your "Answer Key."

* **Action:** Run docker-compose up webapp and navigate to http://localhost:3000.  
* **Additional Learning:** [Docker Compose Basics](https://docs.docker.com/compose/gettingstarted/) – If unfamiliar with Docker, this quickstart will help set up the environment.

### 0.4 SQLite Connectivity

Power BI may require an ODBC driver to read `.sqlite` files.

* **Action:** Install [SQLite ODBC Driver](http://www.ch-werner.de/sqliteodbc/) if the native connector isn’t visible.  
* **Additional Learning:** [Connecting to SQLite in Power BI](https://learn.microsoft.com/en-us/power-bi/connect-data/desktop-connect-sqlite) – Official guide on setup and troubleshooting.

---

## Module 1: The Power BI Mental Model

*The "BI Assembly Line"*

Understand these four pillars to avoid 80% of beginner frustration. Dive into each with targeted resources.

1. **Power Query:** Cleaning (The Raw Material) – [Power Query Documentation](https://learn.microsoft.com/en-us/power-query/power-query-what-is-power-query) and [Beginner Tutorial](https://learn.microsoft.com/en-us/power-bi/transform-model/desktop-query-overview).  
2. **Model:** Relationships (The Skeleton) – [Data Modeling in Power BI](https://learn.microsoft.com/en-us/power-bi/transform-model/desktop-relationships-understand) and [Star Schema Guide](https://learn.microsoft.com/en-us/power-bi/guidance/star-schema).  
3. **DAX:** Measures (The Logic) – [DAX Basics](https://learn.microsoft.com/en-us/dax/dax-overview) and [DAX Tutorial](https://learn.microsoft.com/en-us/power-bi/transform-model/desktop-quickstart-dax).  
4. **Report:** Visuals (The Delivery) – [Creating Reports in Power BI](https://learn.microsoft.com/en-us/power-bi/create-reports/desktop-report-view) and [Visualizations Guide](https://learn.microsoft.com/en-us/power-bi/visuals/power-bi-visualization-types-for-reports-and-q-and-a).

> Black Belt Note:  
> Data modeling is process design. A bad model is a defective process that produces bad data. For deeper Six Sigma integration, see [Applying DMAIC to Data Analytics](https://www.isixsigma.com/tools-templates/dmaic/using-dmaic-data-analytics/).

---

## Module 2: Connect & Load (DEFINE Phase)

*Estimated Time: 1-2 Hours*

### 2.1 Get Data

Connect to data sources and load tables – a foundational skill.

* **Source:** workspace/mental_health_demo.sqlite (Import Mode).  
* **Tables to Load:** fact_encounter, dim_date, dim_facility, dim_provider, dim_patient, dim_diagnosis, dim_payer.  
* **Additional Learning:** [Get Data in Power BI](https://learn.microsoft.com/en-us/power-bi/connect-data/desktop-getting-started) and [Import vs. DirectQuery Modes](https://learn.microsoft.com/en-us/power-bi/connect-data/desktop-use-directquery) – Explains modes and when to use them.

---

## Module 3: Power Query Cleanup

*Estimated Time: 1-2 Hours*

### 3.1 Data Hygiene

Clean data prevents "garbage in, garbage out" results in your slicers. Practice transformations.

* **Facility Names:** Apply Trim and Capitalize Each Word.  
* **Provider Names:** Apply Trim to remove trailing spaces.  
* **Data Types:** **Critical.** Ensure all date_key columns (in both facts and dims) are set to **Whole Number**.  
* **Additional Learning:** [Data Cleaning in Power Query](https://learn.microsoft.com/en-us/power-query/data-cleaning) and [Changing Data Types](https://learn.microsoft.com/en-us/power-query/change-type) – Hands-on examples for trimming, capitalizing, and type conversions.

---

## Module 4: The Star Schema

*Estimated Time: 1-2 Hours*

### 4.1 Relationship Wiring

Build a "Star Schema" in the Model View – key for efficient querying.

* **Rule:** Connect dim tables to fact_encounter.  
* **Configuration:** One-to-Many (*:1), Single Direction.  
* **The Date Rule:** You have three dates (Request, Scheduled, Completed). Set **Scheduled** as the "Active" relationship.  
* **Additional Learning:** [Creating Relationships](https://learn.microsoft.com/en-us/power-bi/transform-model/desktop-create-and-manage-relationships) and [Handling Multiple Date Relationships](https://learn.microsoft.com/en-us/power-bi/transform-model/desktop-relationships-date-tables) – Includes active vs. inactive relationships.

---

## Module 5: DAX Measures

*Estimated Time: 2-4 Hours*

### 5.1 Level 1 Metrics

Create these core measures (Right-click Table > New Measure). Start with simple DAX functions.

* Total Encounters = COUNTROWS('fact_encounter')  
* No Show Rate % = DIVIDE([No Shows], [Total Encounters])  
* Avg Wait Days = AVERAGE('fact_encounter'[wait_days])  
* **Additional Learning:** [COUNTROWS Function](https://learn.microsoft.com/en-us/dax/countrows-function-dax), [DIVIDE Function](https://learn.microsoft.com/en-us/dax/divide-function-dax), and [AVERAGE Function](https://learn.microsoft.com/en-us/dax/average-function-dax) – Reference with examples.

### 5.2 The DEFINE Dashboard

* **Visuals:** KPI Cards for primary metrics + Monthly Trend Line.  
* **Verification Gate:** Numbers must match the **Web App** baseline.  
* **Additional Learning:** [Creating Dashboards](https://learn.microsoft.com/en-us/power-bi/create-reports/service-dashboards) and [KPI Visuals](https://learn.microsoft.com/en-us/power-bi/visuals/power-bi-visualization-kpi) – Tutorial on building and verifying visuals.

---

## Module 6: Metric Verification (MEASURE Phase)

*Estimated Time: 2-4 Hours*

### 6.1 Audit

Don’t trust the data blindly. Use the `vw_wait_time_audit` view.

* **Goal:** Ensure mismatch_count = 0.  
* If it doesn’t match, investigate: Are you using the correct date relationship? Are there nulls?  
* **Additional Learning:** [Data Validation Techniques](https://learn.microsoft.com/en-us/power-bi/transform-model/desktop-data-validation) – Focus on auditing relationships and handling nulls.

---

## Module 7: Root Cause (ANALYZE Phase)

*Estimated Time: 3-6 Hours*

1. **Pareto:** Identify the 20% of Facilities causing 80% of No-Shows. – [Pareto Chart in Power BI](https://learn.microsoft.com/en-us/power-bi/visuals/power-bi-visualization-pareto).  
2. **Heatmap:** Matrix of Facility vs Day of Week to find scheduling bottlenecks. – [Matrix Visual](https://learn.microsoft.com/en-us/power-bi/visuals/desktop-matrix-visual).  
3. **Scatter Plot:** Compare Provider Load vs Outcome Scores to find the burnout inflection point. – [Scatter Chart](https://learn.microsoft.com/en-us/power-bi/visuals/power-bi-visualization-scatter).  
* **Additional Learning:** [Root Cause Analysis with Power BI](https://www.sqlbi.com/articles/analyzing-data-with-power-bi/) – Applies to Six Sigma workflows.

---

## Module 8: Scenario Modeling (IMPROVE Phase)

*Estimated Time: 2-4 Hours*

### 8.1 What-If Parameters

Give leaders a "What-If" slider.

* **Goal:** "If we reduce No-Shows by X%, how many more patients can we see?"  
* **Deliverable:** A slider that updates a "Recovered Capacity" KPI.  
* **Additional Learning:** [What-If Parameters](https://learn.microsoft.com/en-us/power-bi/transform-model/desktop-what-if) – Step-by-step creation guide.

---

## Module 9: Monitoring (CONTROL Phase)

*Estimated Time: 3-6 Hours*

### 9.1 Sigma Bands

The dataset includes standard deviation flags ( `Within_1σ` , `Outside_3σ` ).

* **Visual:** Build a trend chart highlighting encounters that fall "Outside 3-Sigma."  
* **Ops Goal:** Shift from "reacting to everything" to "reacting to signals."  
* **Additional Learning:** [Control Charts in Power BI](https://www.sqlbi.com/articles/statistical-process-control-spc-with-power-bi/) and [Conditional Formatting](https://learn.microsoft.com/en-us/power-bi/create-reports/desktop-conditional-table-formatting) – For highlighting anomalies.

---

## Module 10: The Interview Packaging

*Final Deliverable*

Prepare a 7-minute "Executive Walkthrough" covering:

1. **The Architecture:** Star Schema in Power BI.  
2. **The Baseline:** Current state KPIs.  
3. **The Insight:** Pareto/Root cause analysis of No-Shows.  
4. **The Future:** What-if modeling for capacity planning.  
5. **The Control:** SPC-style monitoring for process stability.

**This narrative proves you aren’t just a "chart builder"—you are an Operations Leader who uses BI as a tool for Six Sigma excellence.**  
* **Additional Learning:** [Presenting Power BI Reports](https://learn.microsoft.com/en-us/power-bi/create-reports/service-get-insights) – Tips for executive storytelling.