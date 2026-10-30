# SME Insights - Power BI Dashboard

An independent Power BI portfolio project combining my experience collecting Small Business Survey data with the transformation and visualisation of published survey findings.

![Business Performance overview showing reported turnover, expected turnover and profitability](documentation/images/dashboard-pages/business-performance-overview.png)

## Contents

- [Why I built this project](#why-i-built-this-project)
- [What this project demonstrates](#what-this-project-demonstrates)
- [What users can explore](#what-users-can-explore)
- [Dashboard pages](#dashboard-pages)
  - [Business performance](#business-performance)
  - [Employment](#employment)
  - [Major obstacles](#major-obstacles)
  - [About this survey](#about-this-survey)
- [Data preparation and validation](#data-preparation-and-validation)
- [Data interpretation](#data-interpretation)
- [Tools used](#tools-used)
- [Methodology and project evidence](#methodology-and-project-evidence)
- [Data source](#data-source)

## Why I built this project

During this project, I have worked in data collection for Fieldlocker alongside BMG Research, primarily on the UK Government’s Small Business Survey. I worked on the 2025 survey and am now working on the 2026 survey.

My job is to speak with senior business leaders and persuade them to volunteer their time to go through the survey with me. I ask them the survey questions and probe their responses to help ensure the data is correct and of high quality, while keeping it interesting and, once in a while, being patient while I hear about the frustrations they are having.

One frustration I heard a lot was that they felt nothing was ever done with the data. This is not exactly true, but it is understandable given the focus on large corporations in the UK. These comments and my natural curiosity made me want to find a way to present the published data to those interested in a more digestible format.

As my first portfolio project, it made sense to choose this subject, not only because I have direct experience collecting data for the survey, but also because the published data is presented in a more challenging format. It is pre aggregated, weighted and presented in a semi structured, wide format, so it would put me to the test and help me learn more skills.

I chose the 2021 to 2024 data since the results were presented in a similar format, making the process repeatable. In addition, the 2025 results had not been relased as of yet. Completing this project allowed me to put what I had been learning through Power BI courses into something real, demonstrating my capability and willingness to learn and adapt.

## What this project demonstrates

| Area | Evidence in the project |
|---|---|
| **Power Query and data preparation** | Prepared one year first and used it as a repeatable transformation template for the remaining years, reshaping the published wide tables into one combined table covering 2021 to 2024. |
| **Reusable query design** | Used a repeatable Staging → Base → Results → Validation structure, then adapted it for different survey questions and annual source tables. |
| **Data modelling** | Built shared business group and year dimensions so that the same selections could filter multiple subject areas consistently. |
| **DAX** | Created percentage point balances, latest year comparisons, dynamic KPI text, selected group context and conditional formatting measures. |
| **Data validation** | Compared transformed results with the published Excel tables and created validation queries to check outcome counts, survey bases and percentage totals. |
| **Report and UX design** | Designed overview and detailed analysis pages, consistent navigation, hierarchical filtering and plain language explanations of the calculations. |
| **Survey interpretation** | Retained weighted results and respondent bases while making the limitations of aggregated survey data visible to the user. |

## What users can explore

- Explore survey responses across the four years and understand the trends and changes in three areas: business performance, employment and major obstacles.
- Use hierarchical slicers to filter the results from the UK total down to individual nations, business sectors or business sizes.
- Separate future expectations from reported outcomes, highlighting where expectations were more positive across business groups in the Employment and Business Performance sections.
- Use percentage point calculations to quickly identify whether increases or decreases outweigh one another and how the gap between the two changed over time.
- Explore the ranking of reported major obstacles for your selected business group and year.
- Track a specific major obstacle and follow how its reported percentage changed over time.
- Track the reported percentage of a specific major obstacle over time and compare it across nations, business sizes or sectors.
  
## Dashboard pages

### Business performance

#### Business Performance overview

The opening page brings together three separate survey questions: reported turnover, expected turnover and profitability. Dynamic KPIs, conditional colours and trend visuals update with the selected business group, giving the user a quick view of the latest position before they move into the detailed analysis.

#### Turnover: the trend behind the headline

![Detailed turnover page comparing reported and expected balances with their underlying response shares](documentation/images/dashboard-pages/turnover-trend-detail.png)

This page compares reported and expected turnover balances across the four survey years. The supporting charts show the increase and decrease shares behind each balance, making the percentage point calculation visible rather than presenting it as an unexplained headline figure.

### Employment

#### Employment Trends and Outlook

![Employment overview separating reported workforce change from expectations for the following 12 months](documentation/images/dashboard-pages/employment-overview.png)

This overview deliberately separates recent employment changes from expectations for the following 12 months. It combines trend measures, a latest year outlook and the distribution of expected responses while keeping the selected business group visible throughout the page.

#### Reported Employment Change

![Detailed employment page comparing reported and expected balances and response distributions](documentation/images/dashboard-pages/employment-response-detail.png)

The detailed page allows the user to compare reported and expected employment balances across the available years and inspect the individual Increased, Stable and Decreased response shares. This helps show the difference between business sentiment and the outcomes subsequently reported.

### Major obstacles

#### Major obstacles: rankings and trends

![Major obstacles page combining annual rankings with a selected obstacle trend](documentation/images/dashboard-pages/major-obstacles-rankings-trends.png)

This page ranks the obstacles reported by a selected business group for a chosen year and follows one selected obstacle across 2021-2024. Dynamic cards highlight the most reported and fastest rising obstacles, while year buttons and an obstacle selector support further exploration.

#### Compare Business Groups

![Comparison page ranking business groups for a selected year and obstacle](documentation/images/dashboard-pages/compare-business-groups.png)

The comparison page lets the user choose a year, obstacle and comparison type before ranking nations, business sizes or sectors. The highest and lowest reported groups are summarised above the chart so that the main result can be understood quickly.

### About this survey

The report also contains an information page explaining the survey source, collection method, weighting, percentage point calculations, respondent bases and interpretation limits. It links back to the official reports and methodology so that users can move from the dashboard to the published evidence.

## Data preparation and validation

Near the end of each year, the results from the Small Business Survey are published on GOV.UK. Each year is split into two sections: businesses with employees and businesses without employees. Within each section are four Excel workbooks. The first is the main workbook, containing the core questions answered by everyone who participated, while the other three are cohort workbooks containing survey questions that were only presented to some respondents selected at random. This helped avoid burdening respondents with too many questions, as the interview already took over 20 minutes to complete using only the core questions.

For each year, I downloaded the core Excel workbook labelled “data” and the Cohort B workbook, as this contained the survey questions for Major Obstacles. This was the topic I found people to be most engaged by, which is why I chose to include Major Obstacles as a section in my own report. The workbook labelled “data” contained the rest of the measures required for the Business Performance and Employment sections.

The published tables were arranged in a wide format, with survey responses in rows and UK, nation, business size and sector results across separate columns. Although this format was useful for reading the published results in Excel, it required reshaping before it could support interactive Power BI filters and comparisons.

I prepared one year first and used it as a repeatable transformation template for the remaining years. In Power Query, I filtered for the required survey table, expanded the nested data, removed unused columns and unpivoted the business breakdown columns. I then standardised the business breakdowns and published responses, separated the percentage results from the weighted and unweighted bases, and created validation queries before appending the annual results into one combined table. Selected results from each year were also checked against the original Excel workbooks. The full process is shown in the [Employment Change methodology](documentation/employment-change-methodology.md), which provides the most comprehensive example of these steps.

## Data interpretation

The dashboard uses published, pre-aggregated and weighted survey responses rather than respondent level records. A methodology document is available to download for each year, explaining how the data was prepared and weighted. For example, here is the [2025 technical report](https://assets.publishing.service.gov.uk/media/6abfc6b9c9bd185c481ec977/lsbs_2025_technical_report.ods).

The 2025 methodology states that the individual survey responses were grouped into 336 separate groups, described as “sector within size band within nation”. These same categories appear throughout the published tables and informed how I organised the data into Breakdown Type and Breakdown Value. However, I did not recreate the weighting process myself, as I used the already weighted figures published by the government.

For each group, the researchers compared its share of the total survey interviews with its estimated share of the wider business population, using the Business Population Estimates released yearly by the government. As a simplified example, if medium sized businesses represented 1% of the survey interviews but the BPE estimated that they represented 3% of the business population, respondents in that group would be given more influence in the results, by weighting the Business Groups.

The published data included two rows: the unweighted base and the weighted base. The unweighted base reports how many businesses answered the question, and the weighted base represents the population base after weighting. The weighted base is not to be understood as additional interviews.

This means the unweighted base is still important. A weighted figure may represent a large number of businesses, but if the unweighted base is small, the result is based on fewer actual interviews and should be interpreted with more caution. I included this warning on the About This Survey page to make it clear to users.

Overall, this dashboard’s purpose is to present the findings published by the government in a more accessible format. It is not intended to establish why a change happened or replace the official reports.

## Tools used

- Power BI Desktop
- Power Query and M
- DAX
- Microsoft Excel
- Git and GitHub

## Methodology and project evidence

- [Business Performance methodology](documentation/business-performance-methodology.md)
- [Employment Change methodology](documentation/employment-change-methodology.md)
- [Employment Expectations methodology](documentation/employment-outlook-methodology.md)
- [Major Obstacles methodology](documentation/major-obstacles-methodology.md)

These documents contain the detailed Power Query steps, validation evidence, DAX examples and design decisions behind each part of the report. The README is intended to remain a shorter overview that can be scanned quickly.

## Data source

The project uses the published SME employer tables available through the UK Government's [Small Business Survey reports collection](https://www.gov.uk/government/collections/small-business-survey-reports), including the [2024 businesses with employees publication and downloadable data](https://www.gov.uk/government/statistics/small-business-survey-2024-businesses-with-employees).

