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
All the pages use a standardised format, a static title and subtitle with dynamic headings that display the business group selection according to the slicer selection, ensuring context is always shown, with a static text box below displaying the source of the data and a caution that low unweighted bases can cause less reliable results.

The sidebar on the left separates the three sections: Business Performance, Employment and Major Obstacles. Under each section, are the two corresponding pages. Hovering over a page selection will cause the button to go light purple, clicking makes it go purple, this makes the sidebar navigation feel responsive and clean. At the bottom is the About this survey page, which takes the user to a static page with information about the data and how it should be interpreted.

### Business performance

#### Business Performance overview

The Business Performance Overview page uses 3 separate survey measures from 3 separate survey questions: reported turnover, expected turnover and profitability. The aim of this page is to bring a quick understanding of the most recent year’s reported turnover balance, expected turnover balance and profitability result. 

It makes use of plain worded KPIs that use conditional formatting and DAX measures to update according to the Business Group slicer, so users may quickly explore areas of interest without it becoming too analytical. 

It is aimed at the average director or senior leader who has less time and requires quick, painless insights. It is the stepping stone to the second page, which is a deeper dive into reported vs expected turnover over the survey years.

**Methodology:** [Business Performance methodology](documentation/business-performance-methodology.md)

#### Turnover: the trend behind the headline

![Detailed turnover page comparing reported and expected balances with their underlying response shares](documentation/images/dashboard-pages/turnover-trend-detail.png)

The Detailed Analysis page focuses on turnover, specifically the direct comparison between reported turnover on the left and expected turnover on the right. This comparison highlights the difference between what businesses expected their turnover to do over the following 12 months and what businesses reported in the next survey year, often showing more positive expectations. 

The top row visualisations display the balance trend over the four years, using percentage points to measure the difference between the increase and decrease response shares. The visuals on the bottom row show how each balance is calculated and give the user a clear understanding of the actual response proportions, as the percentage point calculation can remove this detail. 

The page also makes use of plain worded KPIs and DAX measures to update according to the Business Group slicer, such as the selection heading that updates according to the slicer slection, allowing users to screenshot pages and keep the context clear.

**Methodology:** [Business Performance methodology](documentation/business-performance-methodology.md)

### Employment

#### Employment Trends and Outlook

![Employment overview separating reported workforce change from expectations for the following 12 months](documentation/images/dashboard-pages/employment-overview.png)

The Employment Trends and Outlook page is intended to be easily understood at a glance. It provides plain worded KPIs that use DAX measures to update according to the business selection.

The left side is the reported employment section, first displaying a plain worded KPI with a breakdown of its calculation below, and secondly displaying a column chart that uses percentage point balances to identify the balance between reported increases vs reported decreases, aimed to help the user understand the trend of survey responses over the four years included. Together, the two sections within reported employment first help the user to understand percentage point calculations before moving onto the wider trend.

The right section is employment expectations, starting with a plain worded understanding of the dominant response backed by data, then the distribution of responses in the selected group, and lastly the percentage point calculation between increases vs decreases reported. Combining these gives an informative understanding of the latest year’s employment expectations without using past years that may not be relevant to the reader.

Overall, this page is aimed to be a quick read and an introduction to the Detailed Analysis page, which uses a similar format to the Business Performance Detailed Analysis.

**Methodologies:** [Employment Change methodology](documentation/employment-change-methodology.md) and [Employment Expectations methodology](documentation/employment-outlook-methodology.md)

#### Reported Employment Change

![Detailed employment page comparing reported and expected balances and response distributions](documentation/images/dashboard-pages/employment-response-detail.png)
The Employment Detailed Analysis page focuses on workforce changes, specifically the direct comparison between reported employment on the left and expected employment on the right. This comparison highlights the difference between what businesses expected their workforce to do over the following 12 months and what businesses reported in the next survey year, allowing users to see where expectations were more positive than reported outcomes.

The top row visualisations show the increase and decrease response proportions for each of the four years. The visuals on the bottom row display the balance trend over the four years, using percentage points to measure the difference between the increase and decrease response shares. Together, these give the user a clear understanding of the responses behind each balance, as the percentage point calculation can remove this detail.

The page also makes use of plain worded KPIs and DAX measures that update according to the Business Group slicer, such as the selection heading that updates according to the slicer selection, allowing users to screenshot pages and keep the context clear.

**Methodologies:** [Employment Change methodology](documentation/employment-change-methodology.md) and [Employment Expectations methodology](documentation/employment-outlook-methodology.md)

### Major obstacles

#### Major obstacles: rankings and trends

![Major obstacles page combining annual rankings with a selected obstacle trend](documentation/images/dashboard-pages/major-obstacles-rankings-trends.png)

The Major Obstacles Rankings and Trends page is divided into two sections, visually separated to reduce confusion. Both sides use the same Business Group slicer; however, the left side has an additional year filter. The left side focuses on the chosen year, displaying the ranking of major obstacles reported for that year and selected group. The idea is that the user may see how obstacles move up or down over time and understand what the biggest issue was at the time. For example, for the UK total in 2021, COVID-19 was reported to have been the largest obstacle, during a period when many companies took Bounce Back Loans to survive. By 2024, Taxation was the largest obstacle, increasing from 36% to 60%, a rise of 24 percentage points.

This is my favourite visual, as it tracks what small businesses are feeling and directly expressing as their issues, allowing people to understand how this sentiment changes over time. The right side focuses on the trend of a specific major obstacle over the four years. I wanted users to read the left side, decide what they were curious about, move to the right and select the obstacle they were interested in to see how the sentiment changed over time.

Both sections have a KPI above. The left displays the most reported obstacle for the selected year, while the right displays the fastest-rising obstacle for the selected group. This is worked out using a complex DAX measure created with the help of AI, which calculates each major obstacle’s positive difference from 2021 to 2024, then selects the maximum for the group. It displays the percentage point increase from 2021 to 2024 and the title of the obstacle, which turned out to be Taxation for the UK total.

**Methodology:** [Major Obstacles methodology](documentation/major-obstacles-methodology.md)

#### Compare Business Groups

![Comparison page ranking business groups for a selected year and obstacle](documentation/images/dashboard-pages/compare-business-groups.png)

Similar to the Rankings and Trends page, the Compare Business Groups page uses a ranked bar chart, but instead of ranking obstacles in the selected business group, it ranks business groups by their proportion reporting the selected obstacle. This allows users to understand which business groups reported the selected obstacle the most for the selected year.

In the top right, there is a Comparison Setup box containing 3 slicers. The first is the Breakdown Type whose subcategories you would like to compare, followed by the year you would like to focus on and, finally, the obstacle you would like to rank by. Above the chart are two KPIs that update with these selections, displaying the business groups that reported the selected obstacle the most and least. This gives the user a quick understanding before looking through the full ranking.

For example, if I were to select Sector, 2021 and COVID-19, I would be presented with the ranking and find that the Accommodation and Food industry reported the highest percentage, with 88% of respondents in that group listing COVID-19 as a major obstacle. This can be understood given how much this industry suffered during COVID-19.

**Methodology:** [Major Obstacles methodology](documentation/major-obstacles-methodology.md)

### About this survey

The About This Survey page is a static information page. It helps users understand the data, where it comes from, how it was collected and how it should be interpreted before taking the results as literal survey responses rather than weighted ones from the published reports.

The information is separated into 6 equally sized boxes covering what the survey is, how the responses were collected, how the data is weighted, how the published tables were prepared for the report, what certain calculations mean and how to interpret the findings.

The page’s main aim is to explain how the dashboard uses published, weighted and pre-aggregated survey results rather than individual responses, presenting links to the published reports and the most recent methodology so users can understand the source if they would like to.

**Official methodology:** [2025 Small Business Survey technical report](https://assets.publishing.service.gov.uk/media/6abfc6b9c9bd185c481ec977/lsbs_2025_technical_report.ods)

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

