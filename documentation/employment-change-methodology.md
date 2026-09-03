## Preview

<img width="3038" height="1188" alt="Column_Chart_Net_Measire_PP_Format" src="https://github.com/user-attachments/assets/fc271218-b39f-41c1-bd62-020358d6cec8" />

## Objective

For this part of the dashboard, I wanted to understand how the employment position of UK SMEs changed between 2021 and 2024. I also wanted users to be able to explore whether the overall trend was different across nations, business sizes and sectors.

The published reports contain employment results and statistical breakdowns, but it can take time to find and compare the information relevant to a particular user. By bringing the results into an interactive dashboard, someone interested in a specific sector or business group can find its trend more quickly.

For example, while testing the sector filter, I noticed that the Administrative and Support sector moved from a positive net employment balance of 19 percentage points in 2021 to a negative balance of 7 points in 2024. This does not explain why the change happened, but it highlights a pattern that could be important to businesses or policymakers and may be worth investigating further.

I also created a net employment balance to make the results easier to understand. The original response chart shows the separate trends, but the user still has to compare reported increases and decreases mentally. The balance makes it immediately clearer which one outweighed the other in each year.

## Data Source

I chose to focus on the Small Business Survey results for SME employers businesses with between 1 and 249 employees. The government publishes separate results for businesses without employees, but the employers dataset was more relevant to the questions I wanted to explore, particularly changes in employment and differences between business sizes and sectors.

The dashboard currently uses published results from 2021 to 2024. I selected these years as the first stage of the project because their tables were sufficiently compatible to create a consistent four year dataset. Earlier years may be added later once the initial dashboard structure is complete.

When I first examined the source files, I found that each year was published in a separate Excel workbook containing a large number of statistical tables. The same survey questions also appeared under different table references between years, making it difficult to identify which tables could be compared.

To manage this, I created a mapping workbook showing where each required measure was located in every annual file. I used AI to assist with the initial search across the workbooks, but I manually checked the suggested table references and question wording against the original sources before using them.

The published tables were arranged in a wide format, with survey responses in rows and UK, Nation, Size and Sector results across separate columns. This structure was readable as a fixed statistical table but required substantial reshaping before it could support interactive Power BI filters and comparisons.

I decided to prepare one year first and use it as a repeatable transformation template for the remaining years. This allowed me to understand each stage before applying the same structure across the full 2021–2024 period.

## Selecting the 2021 Employment Table

I started by creating a folder path parameter pointing to the location of the source workbooks. This meant that if the files were moved later, I would only need to update the parameter rather than change the full file path inside every query. It also kept the source steps shorter and easier to manage.

<img width="2348" height="1218" alt="Data_Source_Parameter" src="https://github.com/user-attachments/assets/680af2bb-1525-4cda-91df-b762885a71c6" />

I initially imported only the 2021 Excel workbook because I wanted to understand and test the full preparation process on one year before repeating it across the others.

The resulting source query was named:

```text
Source_2021_Tables
```

I created a reference from this source and renamed it:

```text
EmploymentChange_2021_Staging
```

Referencing the source allowed the same workbook connection to be reused for other survey measures later, without creating a separate import every time. I also disabled load for the staging query because it was an intermediate preparation table rather than a table needed directly in the final Power BI model.

To locate the employment change results, I filtered the source table list for the question containing:

```text
How many employees did the business have on the payroll
12 months ago across all UK sites?
```

This identified Table 36, containing survey question B2. I then opened the nested table in the Data column, revealing the same published results shown in the original Excel workbook.

The table was presented in a wide, publication friendly format, with survey responses in rows and the UK, Nation, Size and Sector breakdowns across separate columns.

<img width="3069" height="1090" alt="Wide_Format_Table_PowerBI" src="https://github.com/user-attachments/assets/ce652113-4adb-4d51-9a5e-bc8982a60b09" />

Steps I had taken in power query:

1. Created a reusable folder path parameter.
2. Imported the 2021 Excel workbook.
3. Referenced the annual source query.
4. Renamed the reference as a staging query.
5. Disabled load for the intermediate staging query.
6. Filtered the named table list to Table 36.
7. Opened the nested table from the Data column.

## Reshaping the Published Table

The original table was difficult to use in Power BI because each UK, Nation, Size and Sector breakdown was stored in a separate column. This meant that England, Scotland, Micro businesses and every individual sector were all separate fields rather than values within one consistent Breakdown field.

Although the format was useful for reading the published results in Excel, it would have made filtering, combining annual tables and writing reusable DAX measures much more complicated.

I kept the first column containing the published response fixed and used **Unpivot Other Columns** on all the business-breakdown columns.

```text
Transform → Unpivot Columns → Unpivot Other Columns
```

Power Query converted the separate breakdown columns into two new fields:

```text
Attribute = the published breakdown, such as Nation England
Value     = the published result, such as 5,648 or 34
```

I then renamed them to make their purpose clearer:

```text
Attribute → Published Attribute
Value     → Published Value
```

The result contained one row for each combination of published response and business breakdown. This made it possible to create consistent Breakdown fields that could later support one slicer across UK Total, nations, business sizes and sectors.

This stage was quite new to me because my previous Power BI and SQL work had used data that was already organised into relational, row based structures. Here, more of the work involved understanding the source layout and reshaping it before I could begin modelling or creating measures. It reinforced that different datasets require different preparation approaches rather than one fixed process.

<img width="3012" height="1278" alt="Unpivoted_Breakdown_Columns" src="https://github.com/user-attachments/assets/5439b547-4e04-4413-b99c-5dc5d4b44b59" />

## Standardising the Business Breakdowns

Before splitting the breakdown labels, I added three fields that would preserve the context of each result after combining the annual tables:

```text
Year            = 2021
Measure         = Employment Change
Table Reference = Table 36
```

The Year identifies the survey period, Measure identifies what is being analysed and Table Reference provides traceability back to the original workbook. When repeating the process for another year, the Year and Table Reference values would need to be updated.

The unpivoted breakdown field still contained combined labels such as:

```text
Nation England
Size Micro
Sector C
```

I wanted separate `Breakdown Type` and `Breakdown Value` fields so that the Power BI filter could organise the results into clear groups:

```text
UK
Nation
Size
Sector
```

Before splitting the field, I replaced `Total` with `UK Total`. This ensured that the UK level result followed the same Type–Value structure as the other breakdowns:

```text
Breakdown Type  = UK
Breakdown Value = Total
```

Without this change, `Total` would not have contained a separate value after the split.

I then used **Split Column by Delimiter**, selecting a space as the delimiter and splitting at the left-most occurrence.

```text
Transform → Split Column → By Delimiter
Delimiter: Space
Split at: Left-most delimiter
```
<img width="3049" height="1777" alt="Split_Column_By_Delimiter" src="https://github.com/user-attachments/assets/4fdc249a-cc0c-4531-a66a-90004e58d3a1" />

Splitting only at the first space was important because it preserved multi word values such as `Northern Ireland`.

The resulting structure was:

| Original label | Breakdown type | Breakdown value |
|---|---|---|
| UK Total | UK | Total |
| Nation England | Nation | England |
| Nation Northern Ireland | Nation | Northern Ireland |
| Size Micro | Size | Micro |
| Sector C | Sector | C |

This structure made filtering easier and created a reusable format that could be applied consistently across the other survey years and measures. The sector codes were translated into readable names later using a lookup table.

## Interpreting and Standardising the Published Responses

The original response labels were not immediately clear enough to use in the dashboard because they only made sense when read alongside the full survey question.

The question asks how many employees the business had 12 months ago compared with its current employment. This means the direction must be interpreted from the business’s position today:

| Published response | Interpretation | Standardised label |
|---|---|---|
| Fewer employees 12 months ago | The business has more employees now | Increased |
| The same number | Employment has not changed | Stable |
| More employees 12 months ago | The business has fewer employees now | Decreased |
| Don’t know | The direction cannot be determined | Unknown |
| Unweighted Bases | Actual number of survey responses | Unweighted Base |
| Weighted Bases | Base adjusted through survey weighting | Weighted Base |

Using the published outcome labels without their full context could have reversed the meaning of employment increases and decreases.

I used **Add Column → Conditional Column** to create a standardised response field. Each published response was mapped to a shorter and more consistent label:

```text
Increased
Stable
Decreased
Unknown
Unweighted Base
Weighted Base
```

I also included the weighted and unweighted base rows in the mapping, removing the final `s` from their published labels to make the naming consistent.

Including every source category ensured that the new column did not contain unexplained null values. This made it easier to check that every published row had been recognised before separating the percentage outcomes from the supporting base records later in the transformation.

The standardised labels were more suitable for charts, filtering and calculations such as the net employment balance.

I kept the original `Published Response` field alongside the new standardised field. This preserved traceability to the source, allowed me to verify that each response had been mapped correctly and made it easier to investigate errors later.

This step reinforced the importance of understanding the wording and time direction of a survey question before presenting its results. A technically correct transformation could still produce a misleading dashboard if the meaning of its categories was interpreted incorrectly.

## Preparing the Published Values for Analysis

The `Published Value` column contained both numbers and the text value `low`, so I could not convert the whole column directly into a numeric data type. Power Query would have returned errors whenever it reached a text value.

In the published tables, `low` means that the result was below 0.5 percent. It does not necessarily mean that no businesses selected that response, so replacing it with zero could misrepresent the source data.

I first created an `Is Low` custom column. This cleaned the text using `Text.Trim` to remove any extra spaces and `Text.Lower` to make the capitalisation consistent. It then checked whether the cleaned value was equal to `low`.

```powerquery
Text.Lower(Text.Trim([Published Value])) = "low"
```

This produced a true or false value for every row. I then used it to create a separate `Numeric Value` column:

```powerquery
if [Is Low] then null
else try Number.FromText([Published Value]) otherwise null
```
<img width="1772" height="1070" alt="Created_Numeric_Value_Custom_Column" src="https://github.com/user-attachments/assets/38a9db1f-4af6-457f-92d9-83320921809b" />

If `Is Low` was true, the numeric value was left blank. If it was false, Power Query attempted to convert the published value into a number. The `otherwise null` part also prevented unexpected text from creating a query error.

I then created a `Row Type` column to distinguish between three different types of published information:

1. Percentage results, showing the proportion of businesses reporting each employment outcome

2. Unweighted bases, showing the actual number of businesses contributing to the result

3. Weighted bases, showing the statistically adjusted base used when producing the published survey estimates

I created separate `Percentage Value`, `Unweighted Base Value` and `Weighted Base Value` columns. Keeping these values separate reduced the risk of accidentally treating a respondent count as a percentage and made the final table easier to understand and use in measures.

The percentage values were divided by 100 and stored as decimals. For example, a published value of `36` became `0.36`. This is the format Power BI expects when displaying a value as 36 percent. If the original value of `36` had been formatted directly as a percentage, Power BI would have displayed it as 3,600 percent.

Finally, I created a `Display Value` column to present the results in a more readable form. This allowed ordinary numeric results to be shown clearly while preserving suppressed values with an understandable label indicating that the result was below 0.5 percent.

This stage helped bring together the full process required to prepare aggregated reporting data. It reinforced that cleaning data is not only about changing its data type. I also needed to understand what each published value represented, preserve uncertainty correctly and create fields that could be used safely in later calculations.

## Creating the Survey Bases Query

I created `EmploymentChange_2021_Bases` as a reference to the completed staging query. The staging query was designed to act as the cleaned foundation for both the Bases and Results queries.

Using a reference meant that any correction made to the staging query would automatically flow through to both dependent queries. Duplicating the query would have created an independent copy, meaning that later corrections might have needed to be repeated manually.

Inside the Bases query, I filtered `Row Type` to keep only:

1. `Unweighted Base`

2. `Weighted Base`

The percentage rows were removed because this query was only intended to prepare the supporting survey bases. I also removed columns that were not needed while retaining the fields required to identify each result:

1. Year

2. Measure

3. Table Reference

4. Breakdown Type

5. Breakdown Value

6. Row Type

7. Numeric Value

At this point, the unweighted and weighted bases still appeared as separate rows. I pivoted the `Row Type` column so that they became two separate fields:

1. `Unweighted Base`

2. `Weighted Base`

I selected `Numeric Value` as the values column because it contained the cleaned number associated with each base. I also selected `Do Not Aggregate` because the government had already published an aggregated base for each business breakdown.

Choosing not to aggregate was also a useful data quality check. If more than one value existed for the same combination of identifying fields, Power Query would expose the duplication instead of silently adding the values together.

After pivoting, each row represented one survey year, measure and business breakdown, supported by both its unweighted and weighted base.

This structure prepared the base information to be joined onto the percentage results later. Each employment outcome could then be displayed alongside the actual number of survey participants contributing to that breakdown and the weighted base used in producing the published estimates.

<img width="2106" height="1320" alt="Pivot_Base_Column" src="https://github.com/user-attachments/assets/942adad6-67f7-484f-8896-80fbda041a25" />

## Creating the Results Query and Joining the Survey Bases

I created `EmploymentChange_2021_Results` as another reference to the completed staging query. As with the Bases query, this meant that any corrections made to the staging query would automatically flow through to the final results.

This time, I filtered `Row Type` to keep only the percentage rows. The base rows were removed because they had already been prepared separately in `EmploymentChange_2021_Bases`.

After filtering, each row represented one employment outcome for a specific survey year and business breakdown. For example:

```text
2021 × Nation × England × Increased
```

I then merged the Results query with the Bases query using the fields that together identified the correct published breakdown:

1. Year

2. Measure

3. Table Reference

4. Breakdown Type

5. Breakdown Value

I did not include the employment outcome in the match because the Bases query did not contain a separate base for Increased, Stable, Decreased and Unknown. One base supported all four outcomes belonging to the same question and business breakdown.

I used a left outer join so that every percentage result remained in the final table. Where a matching base was found, Power Query attached it to the result. If a base was missing, the percentage row would remain visible with a blank base, making the missing match easier to identify during validation.

After completing the merge, I expanded the two required columns:

1. `Unweighted Base`

2. `Weighted Base`

The same base was repeated across the four employment outcomes for each breakdown because all four percentages came from the same group of survey respondents.

This completed the structure required for the annual results table. Each row now contained the published percentage, the employment outcome it represented and the survey bases supporting that result.

<img width="2092" height="1778" alt="Merged_Base+Results_Column_For_Outcome_Base" src="https://github.com/user-attachments/assets/4dec39e9-0a1a-4adc-93f3-312ea08690e7" />

This step reinforced the idea that tables do not always need a single index number to be joined. A combination of fields can act as a composite key, provided that the fields describe the same level of detail in both tables. It also reinforced the importance of understanding what one row represents before deciding how two tables should be connected.

## Ordering the Employment Outcomes

Power BI originally arranged the employment outcomes alphabetically. Although technically correct, this would not have presented the results in the most natural order for the dashboard.

I wanted the outcomes to move logically from positive to negative, followed by responses that could not be classified:

1. Increased

2. Stable

3. Decreased

4. Unknown

I created an `Outcome Sort` conditional column and assigned each outcome a number:

```text
Increased = 1
Stable = 2
Decreased = 3
Unknown = 4
Anything unexpected = 5
```

The final value of 5 ensured that any unexpected category would still appear at the end rather than creating a blank sort value.

I could then use `Outcome Sort` to control the order of the employment outcome field in Power BI. Creating a reusable sort column was more reliable than manually arranging a single visual because the same order could be applied across charts, legends and slicers.

This helped keep the dashboard consistent and reinforced how small modelling choices can make the results easier to follow.

## Validating the Prepared Results

I created a separate validation query because the same checks would need to be repeated across every business breakdown and survey year. Building a repeatable process reduced the risk of overlooking an incorrect row during a manual review.

I grouped the results using:

1. Year

2. Measure

3. Table Reference

4. Breakdown Type

5. Breakdown Value

<img width="1632" height="1272" alt="Validation_Query_Groupby_Setup" src="https://github.com/user-attachments/assets/5c8fad6c-e685-4870-897c-fe5792d15b48" />

Each resulting group represented one published business breakdown containing its possible employment outcomes.

I created several summary checks for each group. The outcome count should normally equal four because the published table contained Increased, Stable, Decreased and Unknown.

I also calculated the sum of the percentage values. The total should be approximately 1, representing 100 percent. I allowed a range between 0.98 and 1.02 because the published percentages were rounded and values marked `low` had been preserved as blank numeric values rather than incorrectly converted to zero.

The same unweighted base should support all four outcomes belonging to a particular breakdown. I therefore calculated both its minimum and maximum value. If they were different, it would suggest that the merge had attached inconsistent bases to the outcome rows.

I created a custom validation column that returned a specific review message when it found one of the following problems:

1. The outcome count was not equal to four

2. The unweighted base was missing

3. The minimum and maximum unweighted bases did not match

4. The percentage total was outside the accepted range

If none of these conditions were found, the group returned `Pass`.

```powerquery
if [Count] <> 4 then "Review: Outcome count"
else if [Minimum Unweighted Base] = null then "Review: missing base"
else if [Minimum Unweighted Base] <> [Maximum Unweighted Base] then "Review: inconsistent base"
else if [Percentage Sum] < 0.98 or [Percentage Sum] > 1.02 then "Review: percentage total"
else "Pass"
```

### Manual Comparison with the Original Source

I did not rely only on the automated validation query. These rules could confirm that the completed table was structurally consistent, but they could not prove that I had selected the correct source table or interpreted every published response correctly.

I selected a sample of results from the completed 2021 query and compared them directly with Table 36 in the original Excel workbook. The checks included:

1. UK employment increased

2. UK employment stable

3. UK unweighted base

4. England employment increased

5. Sector F unweighted base

All five sampled values matched the original published table.

This also tested whether I had interpreted the survey responses correctly. For example, the published response `Fewer (%)` described the number of employees the business had 12 months ago. I confirmed that this correctly appeared as `Increased` in my prepared results because the business had more employees at the time of the survey.

Creating these automated checks and completing a manual source comparison gave me greater confidence that the 2021 transformation could be reused as a template for 2022, 2023 and 2024.

This process reinforced that automated validation and manual checking serve different purposes. The validation query efficiently identified structural problems across the complete dataset, while the manual comparison helped confirm that the source selection, value transformations and response interpretations were correct.

## Repeating the Transformation for Each Survey Year

Once the 2021 process had been prepared and validated, I reused it as the template for 2022, 2023 and 2024.

I first imported each annual Excel workbook as its own source query. I then duplicated the 2021 staging query and renamed each copy for the relevant year.

For every annual staging query, I reviewed and updated the steps that depended on the source year:

1. The source workbook

2. The filtered row containing the correct employment table

3. The Survey Year value

4. The Table Reference value

The remaining transformation steps could be reused because they had been designed to produce the same final structure regardless of the source year.

I then duplicated the 2021 Bases query and changed its source to the new annual staging query. This automatically applied the same filtering and pivoting process to the new year.

I repeated the process for the Results query. I changed its source to the relevant annual staging query and updated its merge step so that it connected to the matching annual Bases query.

Finally, I duplicated the validation query, connected it to the new annual Results query and checked for any rows marked for review. This helped confirm that each annual transformation had produced the expected outcome count, percentage totals and supporting bases.

All four annual Results queries were prepared with the same column names, data types and level of detail. This consistency was necessary because the annual results would later be appended into one combined table.

I disabled loading for the source, staging, base, validation and individual annual results queries. They still refreshed as part of the transformation process, but only the final combined table needed to be loaded into the Power BI model.

<img width="3058" height="930" alt="Annual_Query_Template_Structure" src="https://github.com/user-attachments/assets/d1e53785-0eea-4da4-ab1e-31ca6da1ea81"/>

This stage showed me the value of building and testing a clear template before scaling the process. Giving every Applied Step a meaningful name made it much easier to identify which parts needed updating for each year.

It also made the project easier to extend. When a later survey release becomes available, I can reuse the same structure and add the new year after checking that its question wording and published table remain comparable.

## Combining the Annual Results

Once all four annual Results queries had been prepared and validated, I appended them into one combined table.

An append was the correct operation because every annual query contained the same fields and level of detail. I wanted to place the rows from each year underneath one another while keeping the data in a long analytical format. A merge would have added more columns instead of extending the time period.

I selected `Append Queries as New` and included the Results queries for 2021, 2022, 2023 and 2024. Creating a new query allowed the annual queries to remain as separate preparation stages while the combined query became the final table loaded into the Power BI model.

Power Query matched the annual fields using their column names. This meant that the physical column order was less important, but the names needed to be standardised. A spelling difference would have created an additional column and left blank values in the other years.

Each annual Results query contained 88 rows and 14 columns. The completed append therefore contained:

```text
88 rows × 4 years = 352 rows
14 columns
```

I disabled loading for the four individual Results queries because their data was already included in the combined table. This prevented the model from storing unnecessary copies of the same information.

The operation was similar to using `UNION ALL` in SQL because it combined rows from several compatible datasets without removing repeated values. One difference is that Power Query aligns appended fields using their column names, while SQL unions normally align selected fields by their position.

After appending, I checked selected values from each year against the original Excel workbooks. I compared the UK total unweighted base and a sector unweighted base for each annual source. The sampled Power BI values matched the published values.

<img width="1494" height="1268" alt="Append_Query_Validation_Manual" src="https://github.com/user-attachments/assets/819a246d-ad7e-4863-a5e5-c316c7dc62a7" />

I also checked that each survey year was present and contributed the expected 88 rows. This gave me confidence that none of the annual queries had been excluded or appended more than once.

This stage reinforced my existing understanding of SQL unions while showing me how the same principle is applied through Power Query. It also demonstrated why consistent schemas and source validation are important when combining repeated annual datasets.

## Translating Sector Codes into Readable Names

The published tables used sector codes such as `ABDE`, `C`, `F` and `KL`. These codes kept the original table headings short, but they would have made the dashboard difficult to understand without referring to a separate explanation.

I created a sector lookup table containing two fields:

1. Sector Code

2. Sector Name

<img width="3070" height="1088" alt="image" src="https://github.com/user-attachments/assets/81633eda-af43-4b5e-8cd9-ec89b43a2da6" />

Using a lookup table meant that the code definitions were stored in one place. This was more reusable than adding separate replacement rules to every query and will allow the same sector names to support other measures added later.

I merged the lookup table with the combined Employment Change query by matching:

<img width="1696" height="872" alt="image" src="https://github.com/user-attachments/assets/6c9404f9-1fba-4439-b1c0-ea930b04c5de" />

```text
Breakdown Value = Sector Code
```

I used a left outer join so that every row from the Employment Change table was preserved. Sector rows received a matching name, while UK Total, Nation and Size rows produced a blank `Sector Name` because their breakdown values were not sector codes. These blank matches were expected.

The published code `KL` represented two combined sectors, so I gave it the readable name `Financial, insurance and real estate`.

I then created a custom display column:

```powerquery
if [Sector Name] <> null then [Sector Name] & " Industry"
else if [Breakdown Value] = "Total" then "UK Total"
else [Breakdown Value]
```

This applied three display rules:

1. Sector codes were replaced with their readable sector name followed by `Industry`

2. The value `Total` was displayed as `UK Total`

3. Nation and Size values kept their existing breakdown name

The new display field allowed one slicer and the dynamic chart titles to show understandable labels such as `Construction Industry`, `England`, `Micro` and `UK Total`.

This step reinforced my understanding of lookup tables, left outer joins and conditional display fields. It also showed how separating code definitions from the main results table makes a data preparation process easier to maintain and reuse.

## Creating the Employment Change Trend Visual

For the first visual, I wanted to show how the employment position reported by SME employers changed between 2021 and 2024.

I wanted the dashboard to be understandable to people who may work with businesses or employment policy without having a strong data background. Someone close to me works with a local council and supports policies relating to small businesses and employment. This encouraged me to design the page so that users could investigate the part of the data most relevant to them and identify patterns worth exploring.

I chose a line chart because it could show the direction of each employment outcome across the four survey years.

The visual used:

1. `Year` on the horizontal axis

2. `Published Percentage` on the vertical axis

3. `Employment Change Outcome` as the legend

4. `Survey Base` and the table reference in the tooltip

The legend created separate lines for Increased, Stable and Decreased. I removed Unknown because it provided little useful insight into the direction of employment change and would have added another line to the chart.

### Published Percentage Measure

The source contained percentages that had already been calculated and weighted by the survey publisher. Adding or averaging these percentages inside Power BI could have produced a misleading result.

I created the following measure:

```DAX
Published Percentage =
SELECTEDVALUE(
    'EmploymentChange_2021-2024'[Percentage Value]
)
```

`SELECTEDVALUE` returns the percentage only when the current visual context contains one distinct value. If the user selects several incompatible business breakdowns that produce multiple percentage values for the same chart point, the measure returns blank instead of combining them incorrectly.

The chart can still display Increased, Stable and Decreased together because the legend gives each outcome its own filter context.

### Survey Base Measure

I used the same approach for the unweighted survey base:

```DAX
Survey Base =
SELECTEDVALUE(
    'EmploymentChange_2021-2024'[Unweighted Base]
)
```

I added this measure to the tooltip so users could see how many businesses contributed to the selected result without placing extra information directly on the chart.

### Interactive Business Breakdown

I created a list slicer containing two levels:

1. Breakdown Type

2. Breakdown Value

This allowed users to expand UK, Nation, Size or Sector and then select the particular group they wanted to investigate. The nested structure also made it clearer which type of business breakdown was currently being viewed.

### Dynamic Chart Title

I wanted the title to show users which business breakdown they were currently viewing. I created the following measure:

```DAX
Employment Change Line Chart Title =
"Survey Responses: Reported Employment Change - "
    & [Selected Breakdown Title]
```

The fixed text explains what the chart measures, while `[Selected Breakdown Title]` returns the readable name selected in the slicer.

For example, selecting England changes the title to:

```text
Survey Responses: Reported Employment Change - England
```

Selecting Construction changes it to:

```text
Survey Responses: Reported Employment Change - Construction Industry
```

I applied the measure to the visual using the conditional formatting option for the chart title. As the slicer selection changes, the title updates automatically.

This helps prevent users from forgetting which business group they are viewing and makes screenshots of the visual easier to understand without needing to see the slicer.

The completed line chart made it easy to follow the separate trend of each reported employment outcome. However, it was still difficult to judge whether reported increases outweighed reported decreases in each year.

The chart therefore worked well for exploring the individual responses, but it did not give an immediate summary of the overall direction. This limitation led me to create a second visual showing the net employment balance.

## Creating the Net Employment Balance

The line chart showed the separate Increased, Stable and Decreased trends, but users still had to compare the Increased and Decreased lines mentally to understand which one outweighed the other.

I created separate measures for the Increased and Decreased outcomes.

```DAX
Increased Employment % =
CALCULATE(
    [Published Percentage],
    'EmploymentChange_2021-2024'[Employment Change Outcome] = "Increased"
)
```

```DAX
Decreased Employment % =
CALCULATE(
    [Published Percentage],
    'EmploymentChange_2021-2024'[Employment Change Outcome] = "Decreased"
)
```

`CALCULATE` evaluates `[Published Percentage]` within a modified filter context. The Increased measure filters the outcome field to Increased, while the Decreased measure filters it to Decreased.

The existing context from the survey year and selected business breakdown remains active. This means the measures return the relevant Increased and Decreased percentages for each year and selected nation, size or sector.

I then subtracted the Decreased percentage from the Increased percentage:

```DAX
Net Employment Balance pp =
(
    [Increased Employment %]
    - [Decreased Employment %]
) * 100
```

The result was multiplied by 100 so it could be displayed as percentage points rather than as a decimal. I applied the following custom format:

```text
0.0 "pp";-0.0 "pp";0.0 "pp"
```

<img width="3038" height="1188" alt="Column_Chart_Net_Measire_PP_Format" src="https://github.com/user-attachments/assets/319c1ad1-9087-4cd3-b716-599753045bdd" />

For example, if 34 percent of businesses reported increased employment and 19 percent reported decreased employment, the balance would be:

```text
34% minus 19% = 15 percentage points
```

A positive result means that a greater share of businesses reported an increase than a decrease. A negative result means that a greater share reported a decrease. A result of zero means the two shares were equal.

### Column Chart Design

I chose a column chart because it made the positive and negative values easy to compare across the four survey years. It also gave users a quicker understanding of the overall direction than comparing two separate lines.

I added a zero reference line to show the point where reported increases and decreases were equal. Columns above the line represented a positive balance, while columns below it represented a negative balance.

### Dynamic Column Chart Title

I created a dynamic title so that the chart clearly showed which business breakdown was being viewed:

```DAX
Employment Change Column Chart Title =
"Survey Responses: Net Reported Employment Change - "
    & [Selected Breakdown Title]
```

The measure combines a fixed description with the readable breakdown selected in the slicer. This helps users understand the context of the result and makes exported screenshots easier to interpret.

### Interpretation and Limitations

The measure is a balance of published survey responses. It is not the percentage growth of employment and does not show the number of jobs created or lost.

A business that gained one employee and a business that gained one hundred employees would both be counted as reporting an increase. The measure captures the direction reported by businesses, not the size of each employment change.

The chart also cannot explain why the balance changed. It can highlight a sector or group that moved from a positive to a negative position, but further evidence would be required to investigate the cause.

Changing the display unit to percentage points and including the word `Reported` in the title reduced the risk of users interpreting the result as a measured employment growth rate.

## Employment Change Section Outcome

This section turned four separate years of published survey tables into one reusable dataset covering employment outcomes across UK Total, nations, business sizes and sectors.

The line chart allows users to explore the individual Increased, Stable and Decreased trends, while the Net Employment Balance provides a quicker summary of whether reported employment conditions were more positive or negative in each year.

The interactive breakdown also made it possible to uncover patterns that could be difficult to notice in the published reports. For example, the Administrative and Support sector moved from a positive net balance of 19 percentage points in 2021 to a negative balance of 7 percentage points in 2024. The dashboard cannot explain why this happened, but it makes the pattern visible and provides a starting point for further research.

Completing this section strengthened my understanding of Power Query transformations, lookup tables, validation queries, DAX filter context and visual design. It also helped me apply my previous SQL knowledge to aggregated survey data, which required a different approach from the row level datasets I had worked with before.

The results must still be interpreted carefully. The dashboard uses weighted percentages from published aggregated tables rather than respondent level data. The Net Employment Balance does not measure the number of jobs created or lost, and differences between groups should not automatically be treated as statistically significant.

Overall, this section created a repeatable process that can support additional survey measures and future annual releases. It also established the design approach for the remaining dashboard pages covering business challenges and growth outlook.

* Note this is a design mockup
