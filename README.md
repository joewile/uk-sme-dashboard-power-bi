# SME Insights Power BI Dashboard

> **Status:** Currently in development

## Why I started this project

This project grew from my experience of working on data collection and speaking directly with small businesses. During those conversations, I became interested in whether the issues businesses discussed could also be seen in the published survey data.

Major Obstacles was the subject that received the most attention when I spoke with business owners. They wanted people to understand the problems they were currently facing and felt that these issues needed to be better represented. This made the data feel more personal and meaningful because it records what businesses themselves believe their main challenges are.

My interest in employment data was also influenced by someone close to me who works with a local council and supports work relating to small businesses and employment. This encouraged me to think about how the dashboard could be useful to people working with businesses or local policy who may need understandable findings without having time to search through several large statistical reports.

## Project aim

The UK Longitudinal Small Business Survey contains detailed information, but its results are spread across separate annual Excel workbooks containing many statistical tables. Finding the same question in different years and comparing a particular nation, business size or sector can take time.

I wanted to turn these published tables into an interactive Power BI dashboard that supports two levels of use.

The first is a quick overview for someone who wants to understand the main findings and the latest position included in the dashboard. The second is a deeper investigative view where users can select particular nations, business sizes or sectors, follow changes over time and compare how different groups responded.

One of the main aims is to make trends easier to identify. The dashboard cannot explain why a change happened, but it can highlight a group or result that may be worth investigating further.

## Project scope

The first stage of the project uses published results from 2021 to 2024 for SME employers with between 1 and 249 employees. I selected these years because their tables were sufficiently compatible to create a consistent initial dataset. Earlier years may be added after the main dashboard structure is complete.

The planned report will cover:

- Reported and expected employment change
- Business performance
- Growth and business capability
- Finance and business support
- Major obstacles reported by businesses
- Comparisons between UK nations, business sizes and sectors
- Changes across the 2021–2024 survey period
- Survey-base information and warnings for results supported by small respondent numbers

The dashboard uses published aggregated survey results rather than respondent-level records. It retains the published percentages alongside the weighted and unweighted survey bases.

These figures show what the participating businesses reported. They should not be treated as exact measurements of the full SME population or as proof of what caused a particular result.

## Current development

I am developing the dashboard one survey measure at a time. For each measure, I locate the comparable annual tables, prepare them in Power Query, validate selected results against the original Excel files and append the years into one consistent dataset.

I then create DAX measures and working visuals to test that the filters, calculations and interactions behave correctly.

The current report pages are functional mock-ups rather than finished designs. I decided to complete the data preparation and prototype visuals for all planned measures before applying the final formatting, navigation and page layout. This should make it easier to design the report consistently rather than repeatedly reformatting individual pages while the project is still changing.

## Dashboard pages currently in development

The following pages are working mock-ups. I am currently using them to test the measures, filtering and overall user journey. The final formatting and navigation will be completed after the remaining measures have been added.

### Major obstacle rankings and trends
<img width="1880" height="1056" alt="image" src="https://github.com/user-attachments/assets/ed455977-4c97-4155-a228-be4ddb9bd6ce" />
This page shows the highest reported obstacles for a selected business group and year. The user can also select one obstacle and follow how it has changed between 2021 and 2024. I wanted this page to provide a quick understanding of the main issues while still allowing someone to investigate a specific obstacle in more detail.

### Compare business groups
<img width="1632" height="916" alt="Major_Obstacles_Comparison_Finial_draft" src="https://github.com/user-attachments/assets/77935989-9e4d-4966-8544-a0b7831f5a00" />
This page allows the user to select a year, obstacle and comparison type, then compare the results across nations, business sizes or sectors. It helps show which groups were more or less likely to report a particular issue, making it easier to understand how the experiences of small businesses can differ across the UK.

### Employment Trends and Outlook
<img width="1834" height="1042" alt="image" src="https://github.com/user-attachments/assets/602d41b3-1575-4471-99b0-c84d9a6a23de" />
This overview separates reported employment change from expected employment change to avoid confusing recent experience with future outlook. The left side shows how businesses reported employment compared with 12 months earlier, while the right side shows what they expect over the next 12 months. It is intended for a more casual reader who wants a quick understanding of the employment trend and current business sentiment.

### Reported Employment Change
<img width="1850" height="1042" alt="image" src="https://github.com/user-attachments/assets/71fe96d2-e222-4e18-a372-f4b9847a87e2" />
This page provides a deeper view of employment and employment outlook for a selected business group. It compares reported and expected employment balances while also showing the individual Increased, Stable and Decreased outcomes. This helps the user see how optimistic businesses were and compare that outlook with later reported results, although these remain survey responses rather than actual employment levels.

## Development progress

| Area | Current status |
|---|---|
| Major Obstacles | Finial Draft Completed - Awaiting other measures drafts |
| Reported Employment Outcomes | Data preparation, validation, measures and prototype visuals completed |
| Reported Employment Expectations | Data preparation, validation, measures and prototype visuals completed |
| Remaining survey measures | In development |
| Final dashboard design and navigation | Planned after the measure prototypes are complete |

## Methodology and project evidence

The detailed development logs contain the Power Query steps, validation evidence, DAX measures and screenshots produced while building each part of the dashboard:

- [Employment Outcomes methodology](/documentation/employment-outcomes-methodology.md)
- [Employment Expectations methodology](/documentation/employment-outlook-methodology.md)
- [Major Obstacles methodology](/documentation/major-obstacles-methodology.md)

The main README will remain a shorter overview of the project. The methodology documents provide a more detailed record for anyone interested in how the annual tables were prepared and how the Power BI measures and visuals were developed.
