## Overview of this document
This document is split into two main sections. The first showcases the features of the two Business Performance pages and explains how each page is intended to be read, with the aim of helping the user understand the data provided in the Small Business Survey reports through a visual format. The second explains the technical methodology involved in transforming the semi-structured, wide-format data corresponding to each survey question into appended survey measure tables.

The methodology section will be kept shorter because the full transformation process is already documented in the Employment Change Methodology, which acts as the parent structure for the measures used in Business Performance. This is because most of the survey questions, followed a similar table structure, even when the question wording and response categories differed.
I will use Expected Turnover as the worked example for the three measures, demonstrating the data transformation process and how I reused the query structure from another measure before updating the relevant Applied Steps.
## Business Performance Design
The Business Performance section of the SME dashboard draws on three survey questions, each forming a survey measure used within the dashboard:

- **Reported Turnover:** what businesses reported over the previous 12 months
- **Expected Turnover:** what businesses expected over the following 12 months
- **Reported Profitability:** whether businesses generated a profit or surplus

Given the limitations of the pre-aggregated data, I used a mix of KPIs and visualisations to highlight the key findings behind each survey measure. Together, these provide a concise picture of what businesses in the selected groups are experiencing.
The section consists of two pages, each with its own purpose. The overview provides a quicker and simpler understanding of the main story for users with less time. The detailed analysis page allows for deeper exploration by users who are curious about the data.
#### Shared Business Group Selection
Both pages use a hierarchical slicer based on two columns from a shared dimension table: `Breakdown Type` and `Breakdown Value`. Breakdown Type groups the available selections into UK total, nation, sector and business size, while Breakdown Value contains the individual groups within those categories.
This allows the user to view the UK total, which is the default selection, or switch to a more focused selection that is relevant to them. The selected business-group filter is applied across the whole page, including its KPIs and visualisations.
To provide context, the current selection is also displayed beneath the subtitle using dynamic text. This ensures that the selected group remains clearly identified if a screenshot of the page is taken. On the overview page, the summary banner beneath this provides a plain-language reading of the results.
### Page 1 – Business Performance overview
#### Page layout
The page is divided into three sections, each visually distinguished by a surrounding white background. The top section is designed to guide the reader’s eye from left to right, moving from Reported Turnover to Expected Turnover, before continuing downwards to Profitability. This was inspired by the Z-shaped scanning pattern used in visual design.
#### Section 1 – Reported Turnover
Reported Turnover is divided into two sections: Current Year and Trend Snapshot. These are visually separated by a light-grey divider.

The Current Year section is intended to be read first, allowing the user to immediately understand the balance between businesses reporting an increase in turnover and those reporting a decrease during the most recent survey year.
The main KPI uses a DAX measure called `Reported Gap Text`. This generates a dynamic statement showing whether increases or decreases were higher, alongside the absolute percentage-point gap between them. A separate DAX measure is used for conditional formatting, displaying the text in green for a positive balance and red for a negative balance.
Below this is a plain-language summary of the calculation, generated using the `Reported Response Text` DAX measure. Its purpose is to help users understand how the percentage-point gap was calculated without relying on a technical explanation.
The Trend Snapshot uses a column chart to illustrate changes in the percentage-point balance over time. This provides users with a quick understanding of the overall direction of the reported turnover trend.
#### Section 2 – Expected Turnover
Expected Turnover is divided into two sections: Latest Year Snapshot and Percentage Point Calculation Breakdown. These are also separated by a light-grey divider.
Similar to Reported Turnover, the Latest Year Snapshot uses a DAX measure called `Expected Gap Text`. This generates a dynamic statement showing the absolute percentage-point gap between businesses expecting turnover to increase and those expecting it to decrease over the following 12 months.
Conditional formatting displays the statement in green for a positive balance and red for a negative balance.
The Percentage Point Calculation Breakdown visually shows the response shares used to produce the balance, with the calculation displayed beneath the visual. Its purpose is to make the calculation understandable without relying on technical explanations, helping users interpret the balance visuals on both this page and the detailed analysis page.
#### Section 3 – Profitability
The Profitability section is divided into three visuals intended to be read from left to right. These provide a quick understanding of the latest year’s profitability figures before showing a snapshot of the four-year trend.
The leftmost KPI displays the percentage of businesses reporting a profit or surplus in the latest year, using the `Latest Generated Profit %` DAX measure. Beneath this is a dynamic statement comparing the latest result with the previous year. The `Profit Change Text` measure identifies whether reported profitability has increased or decreased, allowing users to understand the direction of change quickly.
The middle visual uses a stacked bar chart to show the distribution of survey responses for the latest year. Its purpose is to provide a clear view of how the published responses are distributed.
Finally, the line chart shows the percentage of businesses reporting a profit or surplus in each survey year. This allows the user to identify the overall direction and pattern of reported profitability between 2021 and 2024.

### Page 2 – Turnover: the trend behind the headline

#### Purpose and filtering
The Detailed Analysis page’s intention is to allow users to explore the reported and expected turnover measures in more depth, providing a clearer understanding of trends and the data behind the percentage-point balances for each year of the selected group.
The page uses a hierarchical slicer, just like the Overview page. This allows users to select a business group relevant to them, with the selection displayed in a dynamic text box underneath the subtitle. Below this is a dynamic summary which uses DAX measures to update according to the group selection, giving a quick insight into the changes in reported and expected turnover balances.
#### Page structure
The main body of the page is split into a two-by-two structure. The left column shows the Reported Turnover section, while the right shows the Expected Turnover section. This layout allows users to compare reported outcomes with business expectations about turnover in the following 12 months, including expectations from one survey year with the outcomes reported in the following year. For example, comparing 2021 expectations with 2022 reported turnover can highlight the difference between the optimism of businesses and the subsequent outcomes.

#### Percentage-point balance trends
The top row uses column charts to illustrate the percentage-point balance for each year. Its purpose is to show the trend in responses over the years and the size of the gap between businesses reporting or expecting increases and decreases.

#### Response breakdowns
The bottom row uses two clustered bar charts, which act as support for the top row by breaking down the calculation of each year's weighted survey response percentage-point balance. The charts display the percentage of businesses reporting or expecting increases alongside those reporting or expecting decreases.

#### Interpreting the figures
Finally, at the bottom, there is an explanation of how percentage-point balances are calculated. I wanted to make it clear that these figures do not represent actual turnover amounts but differences between survey response shares. This is also made clear on the Information page.

The Employment Change survey measure was the first transformation I completed and set the structure for other measures to follow. It established the general structure:

    Staging → Base → Results → Validation

| Query layer | Steps Taken | Why it was useful |
|---|---|---|
| **Staging** | The Staging query first referenced the annual source table and filtered for the row containing the relevant survey question. It then expanded the nested table, removed unused columns and unpivoted the business-group columns, replacing the one-column-per-group layout with a long-format structure. The published attribute column was split by delimiter into `Breakdown Type` and `Breakdown Value`, which would later support the hierarchical slicer. I then added and standardised fields including `Survey Year`, `Measure Name`, `Table Reference`, `Row Type`, `Response Outcome`, `Is Low`, `Numeric Value` and `Percentage Value`. | Created a consistent long-format structure from the wide published tables, making the data easier to filter, validate and combine across survey years. |
| **Base** | The purpose of this was to create a separate column for `Weighted Base` and `Unweighted Base`, doing this by separating the base rows from the percentage results. Then pivoting its own columns, making one base record for each year and business group. | Preserved the sample information behind each percentage and allowed the correct weighted and unweighted bases to be attached to every result. |
| **Results** | The Results query layer referenced the Staging query, filtered for percentage rows, removed the base-related columns and merged with the Base query using a composite key. It then expanded the merged query, removed duplicates, standardised the employment outcome responses, ordered the columns and set the final data types. | Converted the transformed survey responses into a consistent, model-ready annual table containing the published percentages, standardised outcomes and supporting sample bases. |
| **Validation** | Referenced the completed Results table and grouped the rows by the composite key columns, creating one validation record for each business group. It counted the outcomes, calculated the percentage sum, found the minimum and maximum unweighted bases, and added an `Error Validation` column. If any of the following checks failed, the record would not pass: outcome count = 4, minimum base = maximum base, and percentage total = 1.00 ± 0.02 to allow for rounding. | Provided an automated check that each business group contained the expected responses, consistent bases and a sensible percentage total before the data was used in the report. |

These query layers were then used as the foundation for all my measures, including Major Obstacles, which had a slightly different source layout. Each survey year 2021, 2022, 2023 and 2024 had its own set of four query layers. The completed annual Results queries were then appended to create the final 2021–2024 results table.

### Applied Steps updated when the queries were reused

Although the main transformation logic could be reused, the source-dependent and measure-dependent Applied Steps had to be reviewed and updated.

| Query layer      | Applied Steps or query items changed                                                                                                                            | What was updated                                                                                                                                                                                                                                                                                                    |
| ---------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Staging**      | Query name, `Source`, filtered survey question, `Added Measure`, `Added Table Reference`, `Added Year`, outcome mapping, reordered columns and final data types | The query was renamed and connected to the correct annual source. The relevant survey question, measure name, table number and year were updated. The published response labels were then mapped to the correct outcomes for the new question. Steps referencing the renamed outcome column also had to be checked. |
| **Base**         | Query name and `Source`                                                                                                                                         | The query was renamed and its Source step was changed to reference the corresponding new Staging query. The filtering and pivoting logic could otherwise be reused.                                                                                                                                                 |
| **Results**      | Query name, `Source`, Base merge, outcome sort, reordered columns, kept columns and final data types                                                            | The query was connected to the new Staging and Base queries. The outcome sorting was reviewed, and later steps were updated where they referred to a renamed outcome column.                                                                                                                                        |
| **Validation**   | Query name, `Source` and expected outcome count                                                                                                                 | The query was connected to the corresponding Results query. The expected outcome count was changed where the new survey question contained a different number of response categories. The base-consistency and percentage-total checks could be reused.                                                             |
| **Final append** | Query name and `Table.Combine` query list                                                                                                                       | The final query was renamed and updated to combine the four new annual Results queries for 2021–2024.                                                                                                                                                                                                               |

The structural steps generally remained unchanged. These included expanding the nested table, removing unused columns, unpivoting the business-group columns, splitting the breakdown fields, identifying percentage and base rows, preserving `low` values, pivoting the bases and checking the percentage totals.

Reusing these steps saved time while maintaining a consistent structure across the measures. Once the required updates had been made, I checked selected values against the original Excel tables and confirmed that each annual Validation query passed before appending the Results queries.

## Worked example – Expected Turnover

### Why Expected Turnover was selected

Expected Turnover was the final survey measure. It had the clearest set of screenshots demonstrating how I duplicated the query layers from other completed and verified survey measures and updated the Applied Steps accordingly.

### Source question and original format

The first image shows the downloadable 2021 source file and its original layout. The workbook was semi-structured because several survey tables were stacked within the same worksheet. The Expected Turnover question, Table 137, was also presented in a wide format, with the survey responses stored in rows and each UK, nation, business-size and sector group stored in a separate column.

<img width="2958" height="864" alt="01" src="https://github.com/user-attachments/assets/f53e22a5-2f15-4ad9-b9ae-60ce821c29f4" />

The second image shows how the same table appeared after importing the Excel workbook into Power Query, filtering for the Expected Turnover question and expanding the nested table.

<img width="3072" height="756" alt="02" src="https://github.com/user-attachments/assets/eae3f6f8-9ef9-40a4-bef0-21f70c96d2ee" />

### Updating the Staging query
<img width="2958" height="864" alt="01" src="https://github.com/user-attachments/assets/177e05eb-a72b-44a7-a133-c7a481248dbf" />
<img width="3072" height="756" alt="02" src="https://github.com/user-attachments/assets/6b0ef078-8f9f-4af8-817c-03203c2ed36c" />

First, I updated the duplicated Staging query’s name and source, then filtered for the Expected Turnover question. The existing `Expanded Nested Table` step could then expand the selected survey table.
