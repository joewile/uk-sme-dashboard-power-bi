# Business Performance: Report Design and Methodology

> **Development status**
>
> "Add one sentence confirming whether the two report pages and the 2021-2024 datasets are complete, and that this document is still being finalised."

## Methodology

This methodology outlines the purpose of the Business Performance section, the functionality of the two pages, and how the survey data was prepared for visualisation.

It will begin by explaining how the two Business Performance pages function, their intended purpose and how they are intended to be read.

It will then document the process of transforming each year's published findings from a wide and semi-structured data format into a structured table for each survey year.

Finally, it will explain how these annual tables were appended to create a consolidated dataset for each survey measure.

> **Suggested clarification following the updated understanding of the process**
>
> "The transformations were not rebuilt from the beginning for every Business Performance measure. I duplicated the completed Employment Expectations query structure and then reviewed the Applied Steps individually, updating the sources, table references, measure names, response mappings and query dependencies where required."

---

## Report pages

### Business Performance Overview
<img width="2004" height="1156" alt="02_Performance_Overview" src="https://github.com/user-attachments/assets/2f81700c-339f-4d2e-ba92-62a36f332197" />

The Business Performance section of the SME dashboard draws on three survey questions, each forming a survey measure used within the dashboard:

- **Reported Turnover:** what businesses reported over the previous 12 months
- **Expected Turnover:** what they expected over the following 12 months
- **Reported Profitability:** whether they generated a profit or surplus

Given the limitations of the pre-aggregated data, I used a mix of KPIs and visualisations to highlight the key findings behind each survey measure. Together, these provide a concise picture of what businesses in the selected groups are experiencing.

The section consists of two pages, each with its own purpose. The overview gives a quicker and simpler understanding of the story, aimed at those with less time, while the detailed analysis page allows deeper exploration for those who are curious about the data.

Both pages use a hierarchical slicer based on two columns from a dimension table: `Breakdown Type` and `Breakdown Value`. `Breakdown Type` contains four categories - UK Total, Nation, Sector and Business Size - while `Breakdown Value` contains the individual values in those categories.

This allows the user to view the UK total, which is the default selection, or switch to a more focused selection based on what they consider relevant. The selected breakdown filter is applied to the whole page, including the KPIs.

To provide context, the current selection is also displayed under the subtitle in a dynamic text box. This ensures that the selected group is clearly identified when screenshots are taken. Below this, the overview summary provides a summary of the results in plain words.

#### Page 01/06 - Business Performance Overview

The page is divided into three sections, each visually distinguished by a surrounding white background. The top section is designed to guide the reader's eye from left, reported turnover, to right, expected turnover, and then downwards to profitability. This was inspired by the Z-shaped scanning pattern referenced in visual design documents.

#### Section 1: Reported Turnover

Reported Turnover is split into two sections: Current Year and Trend Snapshot, visually separated by a light grey divider.

The current-year section is intended to be read first, allowing for an immediate understanding of the most recent year's balance between businesses reporting an increase in turnover and those reporting a decrease.

The main KPI uses a DAX measure called `Reported Gap Text`, which generates a dynamic statement displaying the absolute percentage-point gap between businesses reporting increases and decreases. It uses another DAX measure as a conditional-formatting function to display the text as green for a positive balance or red for a negative balance.

Below this is a plainly worded summary of its calculation, generated using a DAX measure called `Reported Response Text`. Its purpose is to help users intuitively understand how the percentage-point gap is calculated without technical explanations.

The trend section uses a stacked column chart to illustrate changes in the percentage-point gap over time. The purpose of this chart is to provide users with a quick understanding of the overall direction of the trend.

#### Section 2: Expected Turnover

Expected Turnover is split into two sections: Latest Year Snapshot and Percentage Point Calculation Breakdown, visually separated by a light grey divider.

The Latest Year Snapshot, similar to Reported Turnover, uses a DAX measure called `Expected Gap Text`, generating a dynamic statement displaying the absolute percentage-point gap between businesses expecting increases and expecting decreases.

Conditional formatting is also used to display the text in green for a positive balance and red for a negative balance.

The Percentage Point Calculation Breakdown section visually explains the percentage-point gap calculation, with the calculation displayed beneath the visual. The purpose is to make the calculation understandable without relying on technical explanations, helping users interpret the visuals on this page and the next.

#### Section 3: Profitability

The Profitability section is split into three visuals, intended to be read from left to right. These give users a quick understanding of the latest year's profitability figures before moving on to a snapshot of the four-year trend.

The leftmost KPI displays the percentage of businesses reporting a profit or surplus in the latest year, making use of the DAX measure `Latest Generated Profit %`. Below this is a dynamic statement that compares the current result with the year before and shows whether it is up or down. It uses a DAX measure called `Profit Change Text` and allows users to quickly identify whether reported profitability has increased or decreased.

The middle visual uses a stacked bar chart to demonstrate the distribution of survey answers for the latest year. Its purpose is to give users a clear understanding of how the responses are balanced.

Finally, the line chart illustrates the percentage of businesses that reported a profit or surplus in each survey year, allowing the user to identify the overall direction and pattern of reported profitability over time.

### Business Performance Detailed Analysis
<img width="1986" height="1140" alt="02_Performance_Detailed_" src="https://github.com/user-attachments/assets/1904950c-ddba-4439-ab25-17e5639dc2b7" />

The Detailed Analysis page's intention is to allow users to explore the Reported and Expected Turnover measures in more depth, providing a clearer understanding of trends and the data behind the percentage-point balances for each year of the selected group.

The page uses a hierarchical slicer, just like the Overview page. This allows users to select a business group relevant to them, with the selection displayed in a dynamic text box underneath the subtitle.

Below this is a dynamic summary that uses DAX measures to update according to the group selection, giving a quick insight into the changes in Reported and Expected Turnover balances.

The main body of the page is split into a two-by-two structure. The left column shows the Reported Turnover section, while the right shows the Expected Turnover section. This layout allows users to compare reported outcomes with business expectations about turnover in the following 12 months, including expectations from one survey year with the outcomes reported in the following year.

For example, comparing 2021 expectations with 2022 reported turnover can highlight the difference between the optimism of businesses and the subsequent outcomes.

The top row uses column charts to illustrate the percentage-point balance for each year. Its purpose is to show the trend in responses over the years and the size of the gap between businesses reporting or expecting increases and decreases.

The bottom row uses two clustered bar charts, which support the top row by breaking down the calculation of each year's weighted survey-response percentage-point balance. The charts display the percentage of businesses reporting or expecting increases alongside those reporting or expecting decreases.

Finally, at the bottom, there is an explanation of how percentage-point balances are calculated. I wanted to make it clear that these figures do not represent actual turnover amounts but differences between survey-response shares. This is also made clear on the Information page.

---

## What the measures represent

| Measure | Period described | Purpose in the report |
|---|---|---|
| Reported Turnover | Previous 12 months | Shows whether more businesses reported an increase or decrease in turnover. |
| Expected Turnover | Following 12 months | Shows whether more businesses expected an increase or decrease in turnover. |
| Reported Profitability | Latest reported period | Shows whether businesses reported generating a profit or surplus. |

The published responses are weighted survey response percentages and are not to be taken as actual turnover increase or decrease percentages which is why I used percentage points.

