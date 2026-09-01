# Major Obstacles: Data Preparation and Prototype Visuals

## Preview

<img width="1916" height="1072" alt="Major_Obstacles_Preview1" src="https://github.com/user-attachments/assets/fc3ec584-4e6d-40fc-b90a-18591faa86eb" />
<img width="1750" height="990" alt="Major_Obstacles_Preview2" src="https://github.com/user-attachments/assets/254ce923-4d9e-4afe-9e2b-582bb744e2d2" />

*Prototype Major Obstacles page used to test the prepared data, measures, filters and visual interactions.*

This log covers the preparation of the Major Obstacles results from 2021 to 2024 and the measures used to test them in Power BI. Several stages were similar to the Employment Outcomes process, so I have documented those more briefly here and concentrated on the decisions specific to this measure.

The visuals shown are working prototypes rather than the final dashboard design. I decided to complete mock-up visuals for each planned measure before applying the detailed formatting and navigation consistently across the final report.

## Objective

Major Obstacles received the strongest response when I spoke with small businesses. The people I interviewed wanted the challenges they currently face to be better understood and represented. This made the data particularly meaningful because it reflects what businesses themselves report as the issues affecting them.

The section is intended to support two types of users. Some may only want a quick understanding of the most frequently reported obstacles in a selected year. Others may want to investigate the results in more detail by following one obstacle over time or comparing nations, business sizes and sectors.

This supports one of the dashboard’s main aims: making changes over time easier to identify while still allowing users to investigate the part of the SME population most relevant to them.

## Selecting the source data

As with the Employment Outcomes measure, I used a reusable folder-path parameter rather than placing the complete file path inside every annual query.

<img width="3072" height="1568" alt="Source_Parameter" src="https://github.com/user-attachments/assets/57c8171f-d664-4c5a-8485-5117eca3f40e" />

*Figure 1. The reusable source parameter allows the source location to be updated without editing every query.*

I then selected the published table containing the question about the major obstacles businesses believed they faced.

<img width="2484" height="1184" alt="Source_Table" src="https://github.com/user-attachments/assets/69d006b1-ef2c-4978-b044-f468e6ef93ca" />

*Figure 2. Selecting the Major Obstacles table from the collection of published tables in the 2021 workbook.*

The source presented similar challenges to the Employment Outcomes data. It used a wide publication-style layout, with UK, nation, business-size and sector results stored in separate columns. Percentage results and survey bases were mixed within the same table, some results were published as `low`, and sectors were identified using abbreviated codes.

<img width="3072" height="1396" alt="Major_Obstacles_2021_Wide_Table_Imported" src="https://github.com/user-attachments/assets/5905220f-f757-4413-95a1-6ffb62dad49f" />

*Figure 3. The original wide table was readable as a publication but unsuitable for reusable Power BI filters and annual appending.*

## Reshaping the annual table

I kept the first column containing the response or base category and unpivoted the remaining breakdown columns. This converted the table into a longer structure with one row for each result and business breakdown.

<img width="3072" height="1794" alt="Major_Obstacles_2021_Unpivoted_Breakdown_Columns" src="https://github.com/user-attachments/assets/a922dcea-e5e9-4729-8522-032fcd5a47a3" />

*Figure 4. Unpivoting converted the separate UK, nation, size and sector columns into Attribute and Value fields.*

I then separated the combined breakdown labels into `Breakdown Type` and `Breakdown Value`. For example, `Nation England` became a type of `Nation` and a value of `England`.

<img width="3072" height="1784" alt="Major_Obstacles_2021_Breakdown_Columns_Created" src="https://github.com/user-attachments/assets/c3249e38-a407-4c98-a73d-77935bd435b9" />

*Figure 5. Splitting at the first space preserved multi-word values such as Northern Ireland.*

This step was important because it allowed nations, business sizes and sectors to use the same fields. Without it, each breakdown would have required separate filters or visuals.

The structure later supported two different interactions:

- Users could select a breakdown type and compare the groups within it.
- Users could select an individual nation, size or sector and investigate its obstacles over time.

## Preparing percentages and suppressed values

The published-value column contained ordinary numbers alongside the text value `low`, so it could not safely be converted directly into a numeric data type.

I preserved `low` rather than replacing it with zero. A low result does not necessarily mean that no businesses reported the obstacle; it means the published percentage was below the display threshold. Replacing it with zero would have changed the meaning of the source.

I created separate fields for:

- Numeric value
- Percentage value
- Unweighted base value
- Weighted base value
- Whether the published result was marked as low

<img width="3070" height="1700" alt="Major_Obstacles_2021_Numeric_Value_Columns" src="https://github.com/user-attachments/assets/2c76a971-0bbd-42be-98de-34f13eab6af7" />

*Figure 6. Separating percentages and bases reduced the risk of treating participant counts as percentage results.*

## Preparing and attaching survey bases

I created a separate bases query and retained only the weighted and unweighted base records. I pivoted the base category so that each business breakdown had separate `Unweighted Base` and `Weighted Base` columns.

<img width="3072" height="1752" alt="Major_Obstacles_2021_Bases_Pivoted" src="https://github.com/user-attachments/assets/2f0c79f7-9f0a-451b-9439-748aa5da1dfe" />

*Figure 7. Pivoting produced one base record for each year, question and business breakdown.*

The results query contained one row for every reported obstacle and percentage. I merged it with the bases query using:

- Year
- Measure
- Table Reference
- Breakdown Type
- Breakdown Value

Together, these fields acted as a composite key identifying the correct base for each published result group. Using only `Breakdown Value` would not have been unique and could have attached survey bases to rows where they did not belong.

I used a left outer join so that every percentage result remained visible, even if its base failed to match.

<img width="3072" height="1758" alt="Major_Obstacles_2021_Results_Merged_With_Bases" src="https://github.com/user-attachments/assets/a347dbb2-9138-4344-ad6a-e590312b0e6a" />

*Figure 8. The merge attached the relevant weighted and unweighted survey bases to every obstacle result.*

The unweighted base records the number of survey participants contributing to the breakdown. The weighted base reflects the adjustment used to make the published results more representative of the wider business population rather than the raw mix of respondents.

## Validation

I created row-level checks for missing bases, failed numeric conversions and percentage values outside the expected range.

<img width="3072" height="1568" alt="Major_Obstacles_2021_Row_Validation" src="https://github.com/user-attachments/assets/3dafb902-dd40-4ab3-abc1-ed68d20c04f0" />

*Figure 9. Row validation returned Pass when the result contained the expected values and supporting bases.*

I also created a grouped validation query. This compared row counts and the minimum and maximum weighted and unweighted bases within each result group. A difference between the minimum and maximum values would indicate that inconsistent bases had been attached to obstacles belonging to the same group.

<img width="3066" height="1544" alt="Major_Obstacles_Composite_Groupby_Validation_Step" src="https://github.com/user-attachments/assets/0bc3fcb3-ce7c-40f3-a45c-afe455039da3" />

*Figure 10. Grouped validation helped expose duplicate rows and inconsistent bases across the combined results.*

I manually compared selected Power BI results with the original Excel tables as an additional check. This process was documented in more detail for Employment Outcomes. Future measure logs will record the validation result and any exceptions without repeating the full procedure each time.

## Repeating and combining the annual queries

After validating 2021, I reused its four queries as the template for each remaining year:

1. I duplicated the staging query, changed its source, located the relevant annual question and updated the year.
2. I duplicated the bases query and changed its reference to the new staging query.
3. I duplicated the results query, changed its staging reference and updated its merge to use the corresponding bases query.
4. I compared selected values with the original Excel workbook.
5. I duplicated the validation query and confirmed that all rows passed.

I repeated this process through 2024. It saved time and ensured that every annual results table had the same columns, data types and level of detail.

Once the four annual tables were ready, I appended them into `MajorObstacles_2021-2024`.

<img width="3072" height="1534" alt="Major_Obstacles_Merge_Percentage_With_Base" src="https://github.com/user-attachments/assets/c29f5f81-4380-4c95-aa81-3f990cd1c3ec" />

*Figure 11. Appending the four compatible annual tables produced one dataset covering 2021–2024.*

## Translating sector codes

The published tables used sector codes that would not be meaningful to most dashboard users. I merged a separate sector lookup table onto the combined results.

<img width="3072" height="1662" alt="Major_Obstacles_Merge_Lookup" src="https://github.com/user-attachments/assets/9ffbb95b-004f-47fc-a116-b821c3fa43a7" />

*Figure 12. The reusable lookup translated sector codes into readable names.*

I used a lookup because the same sector codes appear across several survey measures. This avoided building and maintaining another conditional replacement column inside every future query.

## Measures and report interaction

I created supporting measures for the selected published percentage, survey base, obstacle and business breakdown. These were also used to generate contextual titles for the ranking and trend charts.

The published percentage used `SELECTEDVALUE`:

```DAX
MO - Selected Percentage =
SELECTEDVALUE(
    'MajorObstacles_2021-2024'[Percentage Value]
)
```

I used `SELECTEDVALUE` because each chart point should represent one published result. If the filters create more than one incompatible result, the measure returns blank rather than summing or averaging percentages that should not be combined.

I used the same principle for the survey base:

```DAX
MO - Selected Survey Base =
SELECTEDVALUE(
    'MajorObstacles_2021-2024'[Unweighted Base]
)
```

The survey base was added to the tooltip so users could check how many businesses contributed to a result without overcrowding the visual.

I also sorted the breakdown types so they appeared in a logical order within the hierarchical slicer.

<img width="3072" height="1746" alt="Slicers_Breakdown_Type_Sort" src="https://github.com/user-attachments/assets/25bef445-ac9b-4e1b-816b-15b847376a2b" />

*Figure 13. The hierarchical slicer allows users to move between UK, nation, size and sector results.*

The ranked chart answers: **What were the most frequently reported obstacles for this group in the selected year?**

The trend chart answers: **How has one selected obstacle changed over time for this business group?**

For example, COVID-19 and staff recruitment were among the leading reported issues in 2021. By 2022, energy prices had moved higher in the ranking while COVID-19 had moved lower. The dashboard highlights this change in reported priorities without claiming to explain its cause.

The detailed trend view also showed particularly high reporting of COVID-19 within Accommodation and Food. This was consistent with the disruption experienced by pubs, venues and hospitality businesses, although further evidence would be required before making a causal conclusion.

## Limitations and next steps

These results describe the obstacles that surveyed SME employers reported based on their experiences and opinions. They are not independently verified measures of financial performance and should not be treated as proof that an obstacle affected every business within a group.

Some detailed breakdowns have relatively small respondent bases. For example, one Arts and Recreation result was supported by 57 participants. Results from smaller samples may vary more and should not automatically be treated as representative of the entire industry.

Survey weighting improves representation, but it does not remove all uncertainty or mean that every difference is statistically significant. The dashboard should therefore be interpreted as showing what this survey observed and highlighting patterns worth investigating.

In the final version, I plan to add a low-participant warning to the tooltip. This will encourage cautious interpretation when a result is based on a small number of respondents without adding unnecessary information to the main charts.

The current visuals confirm that the dataset, measures and interactions work. Final page design, navigation and detailed formatting will be completed once working mock-ups have been created for all planned dashboard measures.
