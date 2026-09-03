# Employment Expectations and Employment Outlook

> **Development status:** The pages shown here are working mock-ups used to test the data, measures and interactions. Final formatting, navigation and tooltip warnings will be completed after the remaining dashboard measures have been prepared.

## Preview

### Employment Trends and Outlook - overview page

<img width="1858" height="1032" alt="Screenshot 2026-09-04 000700" src="https://github.com/user-attachments/assets/58c15ae9-7d18-4370-af1d-aca60b6a7f1a" />

The overview separates recently reported employment change from expectations for the next 12 months. It provides a quick view of the historical reported balance, the latest employment outlook and the difference between current reports and future expectations.

### Detailed Employment Analysis

<img width="1860" height="1056" alt="Screenshot 2026-09-04 000728" src="https://github.com/user-attachments/assets/a7b855ab-8094-45cd-b998-751291bf3830" />

The detailed page supports further investigation by comparing reported and expected balances and showing the individual Increased, Stable and Decreased outcomes.

The visual design is still being developed. At this stage, these pages were mainly used to test whether the measures, shared filters and supporting explanations worked together correctly.

## Section overview

The employment section brings together two related survey measures:

- **Employment Change (EC):** how businesses reported their employment level compared with 12 months earlier.
- **Employment Expectations (EE):** whether businesses expect their employment level to increase, remain stable or decrease over the next 12 months.

The section is intended for two types of user:

1. Someone wanting a quick understanding of recent employment trends and the current outlook.
2. Someone wanting to investigate how the results differ by nation, business size or sector.

The overview contains the historical net reported employment balance, the latest expected balance, the distribution of employment expectations and the gap between the two balances.

The detailed page allows the user to compare reported and expected balances over time and investigate the Increased, Stable and Decreased outcomes individually.

Both measures are based on survey responses. They should not be interpreted as showing that employment increased or decreased by a particular percentage. They instead show the balance and distribution of what surveyed businesses reported or expected.

---

## Relationship with the Employment Change methodology

This measure presented similar challenges to Employment Change and followed much of the same preparation process. I have therefore kept this log shorter and focused on the steps that were different or especially important.

The repeated process included:

1. Importing the published survey table.
2. Reshaping the wide table into a reusable long format.
3. Separating percentage results from the survey bases.
4. Merging the bases back onto each result.
5. Validating each year.
6. Appending the completed annual queries.

The more detailed explanation of these shared steps can be found in the Employment Change methodology.

---

## 1. Locating the expected employment question

For each survey year, I located the question asking businesses whether they expected to have more, about the same or fewer employees in 12 months’ time.

### Screenshot 1 - 2021 Table 39 B6 source

<img width="3072" height="1814" alt="01_ExpectedEmployment_2021_Table39_B6_Source" src="https://github.com/user-attachments/assets/ab9373a9-21fd-4853-92e1-fe6461bd6a15" />


Screenshot 1 shows the original 2021 question in Table 39 B6. The table contained unweighted bases, weighted bases and four published responses:

- More than currently
- About the same
- Fewer
- Don’t know

The source was supplied in a wide format. The response types were stored as rows, while UK, nation, business-size and sector breakdowns were stored in separate columns.

The question wording was important when mapping the responses. For Employment Expectations:

- `More than currently (%)` became **Increased**
- `About the same (%)` became **Stable**
- `Fewer (%)` became **Decreased**

This differs from Employment Change. The Employment Change question looks backwards and asks how many employees the business had 12 months earlier. Reporting fewer employees in the past means the business now employs more people, so that response is classed as an increase.

Keeping this difference clear prevented the Employment Change and Employment Expectations outcomes from being mapped in the same direction incorrectly.

---

## 2. Converting the source into a long format

I unpivoted the UK, nation, business-size and sector columns so that they could use one consistent structure.

### Screenshot 2 - 2021 long-format transformation

<img width="3072" height="1546" alt="02_ExpectedEmployment_2021_LongFormat" src="https://github.com/user-attachments/assets/f9df20fc-73f9-49bb-9c65-d9db7f1253d4" />

Screenshot 2 shows the result of converting the wide source table into the `Attribute` and `Value` columns.

The Attribute column was then split into:

- `Breakdown Type`
- `Breakdown Value`

This allowed UK Total, nations, business sizes and sectors to be stored in the same fields. Without this step, every breakdown would have remained in a separate column and required separate slicers and measures.

The long format also made it possible to create one reusable business-category slicer later in the model.

---

## 3. Cleaning and standardising the staging query

After reshaping the table, I created the supporting fields needed for the annual result queries:

- `Year`
- `Measure`
- `Table Reference`
- `Breakdown Type`
- `Breakdown Value`
- `Row Type`
- `Employment Outcome`
- `Is Low`
- `Numeric Value`
- `Percentage Value`

### Screenshot 3 - cleaned 2021 staging query

<img width="3072" height="1542" alt="03_FutureEmployment_2021_CleanedStaging" src="https://github.com/user-attachments/assets/5da506b3-01fd-4c7f-b2f0-c3acb6c2cbce" />

Screenshot 3 shows the cleaned staging structure. Base rows retain their numeric values, while published percentage rows are converted into the separate `Percentage Value` field.

Published whole percentages were divided by 100 so that they could be used correctly in Power BI measures and percentage formatting.

Where a source value was shown as `low`, I preserved that information rather than replacing it with zero. Replacing it with zero would suggest that no businesses selected the response when the source was actually describing a small or suppressed value.

---

## 4. Separating the survey bases

The unweighted and weighted survey bases were separated from the percentage outcomes.

The base rows were filtered into their own query and pivoted so that each business breakdown contained:

- `Unweighted Base`
- `Weighted Base`

### Screenshot 4 - pivoted 2021 survey bases

<img width="3070" height="1408" alt="04_FutureEmployment_2021_PivotedBases" src="https://github.com/user-attachments/assets/a1a79249-f637-46bc-98bb-4a0061b131ab" />


Screenshot 4 shows one row for each UK, nation, size or sector breakdown, with the two base types stored as separate columns.

The results query was then merged with the base query using the identifying fields for the year, measure, table reference and business breakdown. A left join ensured that every published response retained the correct survey bases.

This meant that the Increased, Stable, Decreased and Don’t know rows for a selected breakdown could all provide the same associated participant base.

The unweighted base shows the number of survey respondents. The weighted base reflects the adjustment made to better represent the business population.

---

## 5. Reusing the established standardisation steps

Several display and sorting steps had already been created for previous result tables. I reused this logic to keep the structure of the dashboard measures consistent.

### Supporting screenshot - copying previous standardisation steps

<img width="3070" height="1546" alt="Copying_Steps_From_Previous_Results_Table_Standardisation " src="https://github.com/user-attachments/assets/1312ef33-b2ec-4816-8010-81dea9d8522e" />

This screenshot shows a previously prepared measure table whose standardisation steps were used as a template. It is included as evidence of the process being reused; the table shown is not the source of the Employment Expectations results.

Reusing the existing logic saved time and ensured that breakdown labels and sorting behaved consistently across the dashboard.

---

## 6. Annual validation

Each annual validation query grouped the data by business breakdown and checked:

- That all four expected response outcomes were present.
- That the unweighted base was not missing.
- That the weighted base was not missing.
- That the minimum and maximum base values matched within each group.
- That the percentages totalled approximately 100%, allowing for published rounding.
- That values had been converted correctly.

### Screenshot 6 - 2021 validation

<img width="3072" height="1800" alt="06_ExpectedEmployment_2021_Validation" src="https://github.com/user-attachments/assets/7e3f6df1-bf15-49c2-a83d-daceb7598c8d" />

Screenshot 6 shows the 2021 grouped validation. The `Pass` result confirms that the expected outcomes were present, the bases were consistent and the percentage totals were within the accepted range.

### Screenshot 7 - 2022 validation

<img width="3072" height="1554" alt="09_ExpectedEmployment_2022_Validation" src="https://github.com/user-attachments/assets/dbbbfc5b-2c01-4dbe-a15c-ed9f0e9c1ec2" />

Screenshot 7 shows the same checks applied to the 2022 results after the annual source and query references had been updated.

### Screenshot 8 - 2023 validation

<img width="3068" height="1556" alt="11_ExpectedEmployment_2023_Validation" src="https://github.com/user-attachments/assets/6602d4c0-e77c-4e30-a278-01c2e143a150" />

Screenshot 8 shows that the 2023 breakdowns also passed the outcome, base-consistency and percentage-total checks.

The same validation process was completed for 2024 before the annual queries were appended. Including these examples shows that validation was repeated rather than being performed only on the first year.

The validation expected four published response rows, including `Don’t know`. The main dashboard visuals focus on Increased, Stable and Decreased, but Don’t know remains in the prepared data for completeness.

---

## 7. Repeating and appending the annual queries

After completing the 2021 preparation, I duplicated the four associated queries:

- `ExpectedEmploymentChange_2021_Staging`
- `ExpectedEmploymentChange_2021_Base`
- `ExpectedEmploymentChange_2021_Results`
- `ExpectedEmployment_2021_Validation`

For each new year, I updated:

1. The source query.
2. The filtered survey question or table reference.
3. The year value.
4. The base-query reference.
5. The merge used by the results query.
6. The source used by the validation query.

I checked selected values against the original Excel workbooks and confirmed that the validation rows passed before moving to the next year.

### Screenshot 9 - appending the 2021–2024 result queries

<img width="3070" height="1790" alt="14_ExpectedEmployment_2021-2024_Appended" src="https://github.com/user-attachments/assets/1620a4c8-ee05-43df-8af5-8a7cb9a39daf" />

Screenshot 9 shows the four validated annual result queries being combined into `ExpectedEmploymentChange_2021-2024`.

Duplicating the original query structure saved time and ensured that each year contained the same fields and data types before the append.

---

## 8. Final prepared table

After appending the annual results, I merged the sector-code lookup and added clearer display and sorting fields:

- `Sector Name`
- `Breakdown Display`
- `Breakdown Type Sort`
- `Business Size Sort`
- `Breakdown Value Sort`

### Screenshot 10 - final prepared 2021–2024 table

<img width="3072" height="1514" alt="15_ExpectedEmployment_2021-2024_FinalPreparedTable" src="https://github.com/user-attachments/assets/6c2b2593-eb40-46ab-8118-eea8baa83d82" />

Screenshot 10 shows the final prepared table with readable sector names, business labels and supporting sort columns.

The sorting fields allow the slicer to display the breakdown groups logically:

1. UK Total
2. Nations
3. Business sizes
4. Sectors

Business sizes can also be shown as Micro, Small and Medium rather than being sorted alphabetically.

Using the existing sector lookup avoided creating another custom mapping for names already used elsewhere in the dashboard.

---

## 9. Creating and checking the DAX measures

### Net expected employment balance

The net expected employment balance subtracts the percentage expecting a decrease from the percentage expecting an increase:

```DAX
EE - Net Expected Employment Balance =
VAR IncreasePercentage = [EE - Expected Increase]
VAR DecreasePercentage = [EE - Expected Decrease]
RETURN
    IF(
        ISBLANK(IncreasePercentage)
            || ISBLANK(DecreasePercentage),
        BLANK(),
        (IncreasePercentage - DecreasePercentage) * 100
    )
```

A positive result means more respondents expected an increase than a decrease. A negative result means more respondents expected a decrease.

The result is measured in **percentage points**, not percentage employment growth.

### Outlook difference

I also created a comparison between the latest expected and reported employment balances:

```DAX
EMP - Outlook Difference pp =
VAR ExpectedBalance = [EE - Net Expected Employment Balance]
VAR ReportedBalance = [EC - Net Reported Employment Balance]
RETURN
    IF(
        ISBLANK(ExpectedBalance)
            || ISBLANK(ReportedBalance),
        BLANK(),
        ExpectedBalance - ReportedBalance
    )
```

The KPI used the following custom format:

```text
+0 "pp";-0 "pp";0 "pp"
```

A result of `+7 pp`, for example, means that the expected balance is seven percentage points more positive than the reported balance. It does not mean that employment is expected to grow by 7%.

Where a measure should return only one published value, I used `SELECTEDVALUE`. If the current filter context contains more than one distinct result, the measure returns blank instead of adding or averaging published percentages.

### Screenshot 11 - DAX measures and verification

<img width="3072" height="1512" alt="16_ExpectedEmployment_DAXMeasuresAndVerification" src="https://github.com/user-attachments/assets/bc93a146-c089-46e3-8cc8-e2d3df1375ad" />

Screenshot 11 shows the measures checked in a table across different business breakdowns and years. This allowed me to compare the unweighted base, expected outcome percentages and net expected balance before using the measures in the dashboard visuals.

---

## 10. Creating the shared employment breakdown

Employment Change and Employment Expectations remain in separate fact tables because they come from different survey questions and describe different periods.

To allow one slicer to filter both tables, I added the same composite breakdown key to each table:

```text
Breakdown Key = Breakdown Type | Breakdown Value
```

### Screenshot 12 - employment breakdown key

<img width="3072" height="1536" alt="17_Employment_BreakdownKey" src="https://github.com/user-attachments/assets/7b3c6332-5cd2-4a96-98ae-12c20b7bff6b" />

Screenshot 12 shows values such as:

- `UK|Total`
- `Nation|England`
- `Size|Micro`
- `Sector|ABDE`

Combining the breakdown type and value prevents similarly named values from different breakdown groups from being treated as the same selection.

### Screenshot 13 - shared dimension queries

<img width="3072" height="1504" alt="18_Employment_SharedDimensionQueries" src="https://github.com/user-attachments/assets/83c4eab6-6fab-46c7-a15b-2e2333f0d891" />

Screenshot 13 shows the distinct `Dim Employment Breakdown` query. It contains:

- `Breakdown Key`
- `Breakdown Type`
- `Breakdown Value`
- `Breakdown Display`
- `Breakdown Type Sort`
- `Breakdown Value Sort`

A separate `Dim Year` query was also created. These dimensions provide shared filter values without combining the two employment fact tables.

### Screenshot 14 - shared dimensions in the model

<img width="3072" height="1442" alt="19_Employment_SharedDimensions_Model" src="https://github.com/user-attachments/assets/6516bed5-eff5-47e2-80b9-c564ba39b136" />

Screenshot 14 shows the one-to-many relationships from `Dim Employment Breakdown` and `Dim Year` to both the Employment Change and Employment Expectations tables.

After creating these relationships, I added the breakdown slicer and confirmed that it filtered both sets of visuals. This downstream filtering allows one selection, such as Accommodation and food, to control both sides of the employment analysis.

---

## 11. Overview-page design

The employment overview keeps reported employment and expected employment visually separate.

The left side shows the historical net reported employment balance. This describes employment at the survey date compared with 12 months earlier.

The right side shows the latest employment outlook. This describes what businesses expect to happen during the next 12 months.

I kept these areas separate because combining them into one headline could confuse a casual reader. The titles and supporting descriptions make it clear that one measure looks backwards while the other looks forwards.

I originally considered adding the full historical Employment Expectations trend to the overview. I created the measures and tested the chart but removed it after weighing historical detail against simplicity. The latest outlook felt more useful for someone wanting a quick understanding of current business sentiment.

The Increased, Stable and Decreased proportions are shown alongside the balance because the net balance can hide the underlying distribution. Two groups could have a similar balance while having very different proportions of businesses expecting employment to remain stable.

The outlook-difference callout then gives a quick comparison between recent reported experience and future expectations.

---

## 12. Detailed employment analysis

The detailed page provides a deeper view of reported employment and employment expectations for the selected business group.

It includes:

- The latest reported employment balance.
- The latest expected employment balance.
- The difference between the two balances.
- Reported and expected balances across the available survey years.
- Separate Increased, Stable and Decreased outcome trends.
- A shared business-category slicer controlling both measures.

The balance comparison gives an idea of how optimistic businesses were in previous surveys and how this compares with reported balances shown in later surveys.

The individual line charts allow the user to investigate the outcome distributions rather than relying only on the summarised balance.

This is a broad comparison rather than a forecast-versus-actual test. The surveys are not necessarily completed by exactly the same businesses each year, and the measures represent survey responses rather than observed employment totals.

---

## Limitations and interpretation

These results represent what surveyed SMEs reported and expected. They are not direct measures of employment levels and should not be presented as proof that employment rose or fell by the displayed amount.

Expected employment is also a measure of sentiment, not a guarantee of what will happen. Businesses may expect growth but later experience different trading conditions.

Some nations, sizes and sectors have much smaller respondent bases than others. Their results may therefore vary more between years and may not represent the whole group accurately. Weighting improves representation but does not remove all uncertainty.

The final dashboard will include the unweighted survey base in tooltips and a warning for low-participant groups. This will help users understand that the dashboard shows what this particular survey found rather than treating every value as an exact representation of the SME population.

The working visuals are intended to confirm that the data, measures and filtering behave correctly. Their final layout and formatting will be completed once the remaining measure mock-ups are finished.
