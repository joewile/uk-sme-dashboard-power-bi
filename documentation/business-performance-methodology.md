# Business Performance: Data Preparation, Measures and Report Design

> **Project status:** The two business performance pages are the completed versions for the 2021 - 2024 data scope. This methodology is to explain and document the steps taken to turn the raw data, provided in a wide and semi structural format, into measures used in the report.

## Report pages

### Business performance

![Business Performance overview showing reported turnover, expected turnover and profitability for a selected business group](<img width="1972" height="1130" alt="02_Performance_Overview" src="https://github.com/user-attachments/assets/a95ade5c-9fe7-4b98-82c2-875d47005e0c" />)
)

The overview brings reported turnover, expected turnover and profitability together on one page. It is intended for someone who wants a quick understanding of the latest position while still being able to see how reported turnover and profitability changed between 2021 and 2024.

The three subjects are related, but they do not measure the same thing. I therefore kept them in separate sections of the page rather than combining them into one overall performance score.

### Turnover: the trend behind the headline

<!--
IMAGE TO ADD
Suggested file: documentation/images/business-performance/02-turnover-detail.png
Source: final screenshot of the page titled "Turnover: the trend behind the headline"
Suggested alt text: Detailed turnover page comparing reported and expected balances with their underlying response shares
-->

The detailed page shows the annual reported and expected turnover balances alongside the increase and decrease shares used to calculate them. This allows the user to move beyond the headline balance and see the responses behind it.

Both pages use the same Business Group Selection so that a user can move between the overview and detailed analysis without changing how nations, business sizes and sectors are selected.

---

## Objective and scope

For this part of the dashboard, I wanted to show three aspects of business performance without making the differences between them unclear:

- what businesses reported about turnover during the previous 12 months;
- what they expected to happen to turnover during the following 12 months; and
- whether they reported generating a profit or surplus.

The source results were spread across separate annual Excel workbooks and were designed as fixed statistical tables rather than analytical datasets. Comparing the same business group across several years required the user to locate the relevant table, identify the correct row and then repeat the process in every workbook.

I prepared the 2021–2024 tables in Power Query and kept each survey question in a separate fact table:

```text
TurnoverChange_2021-2024
ExpectedTurnover_2021-2024
Profitability_2021-2024
```

Keeping them separate preserved the meaning and response structure of each question. Shared dimensions and page measures were then used to give the report a consistent selection and presentation layer.

| Measure | Period described | Main published responses | Dashboard calculation |
|---|---|---|---|
| Reported Turnover | Previous 12 months | Increase, Stable, Decrease | Increase share minus decrease share |
| Expected Turnover | Following 12 months | Increase, Stable, Decrease | Expected increase share minus expected decrease share |
| Profitability | Most recent trading period covered by the survey question | Generated profit or surplus, did not generate profit or surplus | Published share reporting a profit or surplus |

The turnover calculations are percentage-point balances between response shares. They are not turnover growth rates. Profitability shows the percentage of respondents reporting that they generated a profit or surplus; it is not a profit margin or a monetary value.

---

## Preparation approach

The three measures followed the same broad preparation pattern:

1. Locate the relevant table in each annual source workbook.
2. Open the nested published table and retain its source reference.
3. Convert the UK, nation, business-size and sector columns from wide to long format.
4. Split the published breakdown into `Breakdown Type` and `Breakdown Value`.
5. Standardise the published responses while keeping their original labels.
6. Preserve values marked `low` instead of treating them as zero.
7. Separate the weighted and unweighted base rows from the percentage results.
8. Pivot the base rows and merge them back onto the corresponding results.
9. Validate the expected outcomes, survey bases and percentage totals.
10. Append the checked annual results into a consistent 2021–2024 table.

I have shown this process in full for Reported Turnover. Expected Turnover and Profitability reused the same tested structure, so their sections concentrate on the parts that genuinely differed. This avoids repeating the same Power Query steps three times while still documenting the decisions that affected the meaning of the data.

---

## 1. Reported Turnover: full worked example

### Selecting and checking the published table

I began with the published table showing how businesses described their turnover compared with the previous 12 months.

The table contained two different types of information in the same structure:

- weighted and unweighted survey bases; and
- published response percentages for increased, decreased, stayed the same, did not know and refused.

UK Total, nations, business sizes and sectors were stored in separate columns. This made the table readable in Excel, but unsuitable for one reusable Power BI slicer or for appending several survey years.

<!--
FIGURE 1
Suggested title: Original Published Turnover Table
Source file: Turnover Change/2_Checked_Source_Change_Updated_Wide_Table.png
Suggested destination: documentation/images/business-performance/03-original-published-turnover-table.png
-->

*Figure 1. The original turnover table combined response percentages and survey bases in a wide publication-friendly structure.*

### Reshaping and standardising the table

I kept the published response column fixed and unpivoted the remaining business-breakdown columns. This converted the table into a long format containing one row for each response and business group.

The published breakdown labels were then separated into:

```text
Breakdown Type
Breakdown Value
```

For example:

| Published breakdown | Breakdown Type | Breakdown Value |
|---|---|---|
| UK Total | UK | Total |
| Nation Northern Ireland | Nation | Northern Ireland |
| Size Micro | Size | Micro |
| Sector I | Sector | I |

Splitting only at the first space preserved multi-word values such as Northern Ireland. I also retained `Year`, `Measure` and `Table Reference` so that every result could be traced back to its annual source.

The published response labels were mapped to a consistent `Turnover Outcome`:

```text
Increase
Stable
Decrease
Don't know
Refused
```

I kept the original `Published Response` and `Published Value` fields alongside the standardised fields. This made it easier to check how a displayed result related to the source and to investigate any unexpected values later.

Numeric published percentages were divided by 100 and stored in `Percentage Value` so that they behaved correctly with Power BI percentage formatting. Where the source contained `low`, I set an `Is Low` flag and did not convert the value to zero. A suppressed or very small value is not the same as no respondents selecting that answer.

<!--
FIGURE 2 — NEW SCREENSHOT REQUIRED
Suggested title: Cleaned and Standardised Turnover Table
Capture the completed Turnover staging/result query with these columns visible where possible:
Year, Breakdown Type, Breakdown Value, Turnover Outcome, Is Low, Percentage Value, Unweighted Base and Weighted Base.
Suggested destination: documentation/images/business-performance/04-cleaned-standardised-turnover-table.png
Do not use an older screenshot that still displays Profitability query names or values.
-->

*Figure 2. The cleaned table stores business groups and turnover outcomes as values rather than separate columns.*

### Preparing and attaching the survey bases

The weighted and unweighted bases describe the survey question and business group rather than one individual response outcome. I therefore separated the base rows into a supporting query and pivoted them into two fields:

```text
Unweighted Base
Weighted Base
```

The prepared base table was merged back onto the percentage results using the identifying fields for the year, measure, table reference and business breakdown. I used a left outer join so that the published result rows were retained while their matching bases were added.

This gave every Increase, Stable, Decrease, Don't know and Refused row for a business group access to the same supporting bases. It also meant the unweighted respondent count could be shown in report tooltips without adding base values across response outcomes.

The unweighted base represents the number of survey respondents. The weighted base reflects the adjustment used in the published statistics to better represent the business population. I retained both because they answer different questions.

### Automated and manual validation

I created a separate validation query for each annual result table. The results were grouped by year and business breakdown so that the checks could confirm:

- the expected number of response outcomes was present;
- the unweighted base was not missing;
- the weighted base was not missing;
- the base remained consistent across every outcome in the group; and
- the published percentages totalled approximately 100%, allowing for rounding in the source tables.

<!--
FIGURE 3
Suggested title: Automated Turnover Validation Checks
Source file: Turnover Change/10_Updated_validation_Custom_Column.png
Suggested destination: documentation/images/business-performance/05-automated-turnover-validation-checks.png
-->

*Figure 3. The validation column flags missing outcomes, missing or inconsistent bases and percentage totals requiring review.*

The automated checks were useful for confirming structural consistency, but they could not prove that I had selected the correct source question or interpreted its wording correctly. I therefore also compared selected UK, nation, size and sector results with the original Excel tables before combining the years.

<!--
FIGURE 4 — NEW SCREENSHOT REQUIRED
Suggested title: Turnover Validation Results
Capture the final validation query with its result/status column and visible Pass values.
Suggested destination: documentation/images/business-performance/06-turnover-validation-results.png
-->

*Figure 4. The completed validation output provides a clear record that the annual result groups passed the defined checks.*

### Repeating and combining the annual process

Once the first annual query was working, I reused its staging, base, result and validation structure for the remaining years. For each copy I updated and checked:

1. the annual source query;
2. the filtered survey question;
3. the year and table reference;
4. the base-query source;
5. the merge used by the results query; and
6. the validation-query source.

Reusing a tested structure reduced unnecessary work, but I did not assume that the workbooks were identical. Each year's labels, table reference, row count and validation result still needed to be checked.

<!--
OPTIONAL SUPPORTING IMAGE
Suggested title: Reusing the Validated Process for a Later Survey Year
Source file: Turnover Change/11_Duplicated_2021_Steps_Starting_Updating_For_2022.png
Use only if the document needs extra evidence of the repeated annual process.
-->

The four validated annual result queries were then appended into `TurnoverChange_2021-2024`. The combined table received readable sector names, display labels, sort fields and a composite breakdown key.

<!--
FIGURE 5 — BEST AS A TWO-PANEL IMAGE
Suggested title: Appending and Checking the 2021–2024 Turnover Results
Left source: Turnover Change/13_Appended_Updated_Quries.png — crop tightly around the Append dialog.
Right source: Turnover Change/14_Checking_Final_Turnover_Change_Results.png — preferably replace with a clean screenshot without the open filter menu.
Suggested destination: documentation/images/business-performance/07-turnover-append-and-final-table.png
-->

*Figure 5. The validated annual queries were appended into one table while retaining the year and source-table reference for traceability.*

---

## 2. Expected Turnover: what changed

Expected Turnover followed the same wide-to-long, base, validation and append process. The important difference was the meaning of the question.

Reported Turnover describes what businesses said happened during the previous 12 months. Expected Turnover asks what they think will happen during the following 12 months. Treating these as interchangeable would confuse recent experience with business sentiment.

The expected-turnover responses were standardised as:

| Published meaning | Standardised outcome |
|---|---|
| Turnover expected to increase | Increase |
| Turnover expected to stay the same | Stable |
| Turnover expected to decrease | Decrease |
| Don't know | Don't know |
| Refused | Refused |

<!--
FIGURE 6 — BEST AS A TWO-PANEL IMAGE
Suggested title: Expected Turnover Source and Outcome Standardisation
Left source: Expected Turnover Change/2_Expected_Turnover_Wide_Format.png
Right source: Expected Turnover Change/6_Updated_Staging_d_outcome.png
Suggested destination: documentation/images/business-performance/08-expected-turnover-source-and-mapping.png
-->

*Figure 6. The expected-turnover source used a forward-looking response structure, which was mapped separately from reported turnover.*

### Adapting to changed 2024 labels

The 2024 source did not use exactly the same published labels as the earlier workbooks. Labels such as `Increase Col %`, `Decrease Col %` and `Stay the same Col %` required an updated mapping condition.

I changed the annual transformation to recognise the new labels while preserving the same standard outcomes used by the other years. I then checked the prepared rows and validation output before appending 2024.

This was an important reminder that a reusable query is a starting point rather than a substitute for checking the source. Copying the earlier logic without reviewing the 2024 table could have produced missing outcomes or incorrect blanks.

<!--
FIGURE 7
Suggested title: Adapting to Changed 2024 Expected Turnover Labels
Source file: Expected Turnover Change/16_Updated_2024_Condiatonal_Column_For_Published_Respone_Change.png
Suggested destination: documentation/images/business-performance/09-expected-turnover-2024-label-change.png
-->

*Figure 7. The 2024 conditional logic was adapted to recognise changed published labels while retaining a comparable final outcome field.*

---

## 3. Profitability: what changed

Profitability required a different response mapping because it is not an increase-versus-decrease question. The published table recorded whether the business generated a profit or surplus, together with Don't know and Refused responses.

I standardised the source labels into:

```text
Generated profit or surplus
Did not generate profit or surplus
Don't know
Refused
```

The report uses the weighted percentage reporting that a profit or surplus was generated. I did not create a net profitability balance because subtracting the No response from the Yes response would add a second metric without making the underlying published measure clearer.

<!--
FIGURE 8 — BEST AS A TWO-PANEL IMAGE
Suggested title: Profitability Source and Response Mapping
Left source: Profitability/1_Wide_Table_Format.png
Right source: Profitability/4_Updated_Conditonal_Column_Employment_Outcome.png
Suggested destination: documentation/images/business-performance/10-profitability-source-and-mapping.png
-->

*Figure 8. Profitability used a Yes/No-style response structure and was therefore standardised separately from the turnover measures.*

### Adapting the validation rules

The Profitability validation followed the same principles as the turnover checks, but the expected outcome count reflected its own response structure. This was important because copying the turnover rule without changing it could have made a structurally incorrect table appear acceptable or caused valid data to fail unnecessarily.

<!--
FIGURE 9 — TWO-PANEL IMAGE
Suggested title: Profitability Validation Logic and Results
Left source: Profitability/8_Updated_Validation_Column.png
Right source: Profitability/9_Validation_Result.png
Suggested destination: documentation/images/business-performance/11-profitability-validation.png
-->

*Figure 9. The Profitability checks were adapted to the measure's expected outcomes and produced a visible Pass result for the prepared groups.*

### Handling the changed 2024 format

The 2024 Profitability table also used different labels, including values such as `Yes Col %`. I updated the mapping for that annual query and retained the same final Profitability Outcome values used in the earlier years.

This step demonstrates why I checked the annual source structure even when the overall transformation process had already been tested.

<!--
FIGURE 10
Suggested title: Handling the Changed 2024 Profitability Format
Source file: Profitability/11_2024_Different_Published_Outcome_Updated_Conditional_Table.png
Suggested destination: documentation/images/business-performance/12-profitability-2024-format-change.png
-->

*Figure 10. The 2024 response labels were handled explicitly so that the four-year Profitability table remained comparable.*

---

## 4. Combining the measures in the model

The three final fact tables contain comparable business breakdowns but represent different survey questions. I kept them separate and reused the project's existing shared dimensions:

```text
Dim Employment Breakdown
Dim Year
```

`Dim Employment Breakdown` contains a composite key:

```text
Breakdown Key = Breakdown Type & "|" & Breakdown Value
```

This distinguishes values that belong to different breakdown groups and supports a hierarchy from Category to Business Group. It also provides readable display labels and sort fields for UK Total, nations, business sizes and sectors.

The shared dimensions allow the same Business Group Selection and year context to filter the relevant fact tables. This avoided creating separate nation, size and sector slicers for each measure and gave the two pages a consistent interaction pattern.

<!--
FIGURE 11
Suggested title: Shared Dimensions Across the Business Performance Measures
Preferred source: a new clean Model view showing Dim Employment Breakdown, Dim Year and the three Business Performance fact tables.
Existing fallback: Expected Turnover Change/18_Managing_Relationships.png
Suggested destination: documentation/images/business-performance/13-business-performance-model.png
Only state exact relationship cardinality/filter direction if clearly visible in the final screenshot.
-->

*Figure 11. Shared dimensions provide a consistent business-group and year context while the three survey measures remain in separate fact tables.*

---

## 5. Measures and calculations

### Returning one published percentage

The fact tables contain published percentages rather than additive transaction values. I used `SELECTEDVALUE` as the base measure so that Power BI returned a value only where the current filters identified one published result.

```DAX
TC - Selected Percentage =
SELECTEDVALUE(
    'TurnoverChange_2021-2024'[Percentage Value]
)
```

This is safer than summing or averaging percentages across years, business groups or response outcomes. If the context is ambiguous, the measure returns blank rather than producing a misleading total.

### Reported turnover balance

The reported turnover balance subtracts the weighted share reporting a decrease from the weighted share reporting an increase:

```DAX
TC - Net Turnover Balance pp =
(
    [TC - Increased Turnover %]
        - [TC - Decreased Turnover %]
) * 100
```

A positive result means that a larger share reported increased turnover than decreased turnover. A negative result means the opposite. Stable, Don't know and Refused responses remain in the data but are not part of the balance calculation.

Multiplying the difference by 100 presents the result in percentage points. For example, 36% minus 30% is a balance of `+6 pp`, not turnover growth of 6%.

### Expected turnover balance

Expected Turnover uses the same calculation structure with the expected increase and decrease measures:

```DAX
ET - Net Expected Turnover Balance pp =
(
    [ET - Expected Increase %]
        - [ET - Expected Decrease %]
) * 100
```

The result describes the balance of expectations for the following 12 months. It is a measure of survey sentiment rather than a forecast of the turnover businesses will later record.

### Comparing the latest reported and expected balances

The overview also compares the latest expected balance with the latest reported balance:

```DAX
BP - Turnover Outlook Difference pp =
[ET - Latest Expected Turnover Balance pp]
    - [TC - Latest Turnover Balance pp]
```

A positive value means the outlook is more positive than the latest reported position. It does not test whether an earlier expectation became the later result.

### Profitability

The core Profitability measure filters the selected published percentage to:

```text
Generated profit or surplus
```

The latest-year calculation compares this share with the previous survey year and describes it as up, down or unchanged in percentage points. Don't know and Refused are also retained and can be combined into a No Answer measure without treating them as businesses that did not make a profit.

### Context and latest-year safeguards

The overview identifies the latest available year separately for Reported Turnover, Expected Turnover and Profitability. If those years differ, the page text reports them separately rather than implying that all three headlines refer to the same period.

Page measures also check that one `Breakdown Key` is selected. Where the selection is ambiguous or a required value is missing, the report returns a blank or an explanatory message rather than displaying an unreliable calculation.

I created separate measures for:

- dynamic section and chart titles;
- positive, negative and unavailable colours;
- calculation text showing the response shares behind a balance;
- latest-year filtering; and
- unweighted respondent-base text used in tooltips.

This kept the interpretation rules inside the model instead of relying on static labels that could become incorrect when the user changed the selected business group.

---

## 6. Report design decisions

### Business performance overview

The overview was designed to answer the main questions in one scan:

- Did more businesses report turnover increases or decreases?
- Was the latest turnover outlook more positive or negative?
- What share reported generating a profit or surplus?
- How have the reported turnover balance and profitability changed over time?

Reported and expected turnover are presented in separate cards because one describes recent experience and the other describes the following 12 months. The headline wording changes with the sign of the balance, while the supporting text shows the response shares used in the calculation.

I included the historical reported-turnover and profitability trends to provide context without turning the overview into a detailed analysis page. The expected section instead concentrates on the latest response distribution because the detailed page provides the fuller annual comparison.

### Turnover: the trend behind the headline

The detailed page compares reported and expected balances for each available year. Beneath each balance chart, the increase and decrease shares are shown separately.

I included both views because the balance is useful for comparison but can hide the response distribution. Two years could have the same balance while having different proportions of businesses reporting increases, decreases or stable turnover.

The detail measures require one business group and one year where appropriate. Dynamic titles, period text and calculation tooltips help the user keep track of the current context.

The page also states that expectations describe the following 12 months rather than observed outcomes. This distinction is important when reported and expected values are displayed next to one another.

---

## 7. Validation and interpretation

The final checks covered both the prepared data and the behaviour of the report:

- selected results were compared with the original annual Excel tables;
- annual queries checked outcome counts, missing bases, base consistency and percentage totals;
- expected and profitability mappings were updated where the 2024 labels changed;
- the combined tables retained year and table-reference fields for traceability;
- the business-group selection was tested across all three fact tables;
- the DAX balances were checked against their underlying increase and decrease shares; and
- unweighted respondent bases were checked in the relevant filter context.

The following limitations remain important:

- The dashboard uses published aggregated survey tables, not respondent-level records.
- Published percentages are weighted; the unweighted base is the number of respondents supporting a result.
- I retained the published weights rather than calculating new survey weights.
- A turnover balance is a percentage-point difference between response shares, not a rate of turnover growth.
- Expected Turnover represents what businesses anticipated, not a guaranteed forecast or observed later result.
- The Profitability percentage is not a profit margin and does not show the amount of profit generated.
- Values marked `low` are not zero.
- Annual samples may not contain exactly the same businesses.
- Differences between groups or years are descriptive and are not automatically statistically significant.
- The dashboard can highlight a result worth investigating, but it cannot explain what caused the change.

---

## Business Performance section outcome

This part of the project produced three validated 2021–2024 fact tables and two report pages serving different levels of analysis.

The main technical challenge was not simply importing the tables. It was preserving the meaning of three different survey questions while giving them a consistent structure for filtering, comparison and presentation.

Reusing the annual query pattern saved time, but the changed 2024 labels showed why every source still required manual review. Separating the common preparation logic from the measure-specific mappings made the process easier to maintain and reduced the risk of copying an assumption from one question into another.

The finished section demonstrates:

- reshaping wide published tables into analytical long-format data;
- retaining weighted and unweighted survey information;
- adapting transformations to changes between annual sources;
- building reusable validation checks;
- modelling separate facts through shared dimensions;
- writing filter-aware DAX measures and dynamic explanatory text; and
- designing overview and detailed pages for different types of user.

The result is a section that can give a quick headline while still allowing the user to inspect the response shares, respondent bases and annual trends behind it.
