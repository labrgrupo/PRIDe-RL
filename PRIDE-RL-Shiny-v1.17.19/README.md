# PRIDE-RL Shiny

Version 1.17.19

Shiny application for comparing reference limits estimated by two or more methods against comparative reference limits, calculating equivalence intervals according to the PRIDE-RL spreadsheet logic, generating rankings, and exporting the heatmap.

The `app.R` source is organized with an English documentation map and numbered section headers. Each major block explicitly identifies the corresponding Shiny screen or command, including Data entry, Results, Heatmap, PRIDe-RL scoring framework, References, Generate Analysis, and Download HTML Report. Server-side comments also map reactive state, spreadsheet paste, configuration management, calculations, rendering, and downloads to their visible controls.

The responsive header combines the embedded PRIDe-RL logo with the full name **Performance Ranking by Index of Deviation of Reference Limits (PRIDe-RL)** and the subtitle **A multidimensional framework for comparative evaluation and ranking of reference-limit estimation methods**. The 600 dpi source logo is stored directly in `app.R` as a Base64 data URI, so no external image file or runtime image path is required. The interface uses the LabR blue identity (`#0d47a1`, `#0033CC`, and `#4878d5`), with pale yellow (`#FFFEC2`) reserved for supporting guidance.

Every tab ends with a responsive institutional footer containing the developer, LabR Group, GPL-3.0 license, GitHub, and Lab R Group Website information.

The main navigation includes a dedicated **References** tab containing only the complete methodological bibliography. DOI links open the corresponding publication in a new browser tab. The same centralized reference list is reproduced in the generated HTML report.

The **PRIDe-RL scoring framework** tab renders all mathematical expressions with MathJax from LaTeX source delimited by `$$ ... $$`. Fractions, subscripts, absolute-value symbols, inequalities, percentiles, and exponents are displayed as mathematical notation, with horizontally scrollable formula blocks on narrow screens.

The same tab includes the complete PRIDe-RL workflow figure as an embedded Base64 JPEG tagged at 600 dpi. It is centered and limited to 600 px on larger screens while adapting to 100% of the available width on phones and tablets without distortion or an external image file.

A compact glossary immediately below the workflow figure defines its abbreviations and variables. The glossary uses smaller, muted typography and a responsive two-column layout that collapses to one column on phones so the figure and scoring equations remain visually dominant.

Immediately below the formulas for d1, d2, d3, and d4, responsive **Where:** blocks define every variable and relevant fixed term, including the evaluated-limit count, total available limits, inclusive 90th percentile, equivalence boundary, severity threshold, and penalty multiplier.

The scoring dimensions use clean subsection headings and centered formulas inside restrained pale-yellow panels with a darker pastel border. Their **Where:** definitions use the same compact, neutral typography and transparent background as the workflow-figure glossary, avoiding competing colored panels.

The final **PRIDe Score** equation is typeset directly with MathJax as an indexed fourth root, using the notation `PRIDe Score = fourth root of (d1 × d2 × d3 × d4)`. A dedicated compact beige panel distinguishes the composite-score equation from the four dimension equations while preserving responsive horizontal scrolling on narrow screens.

In the initial configuration area, the **Analysis information** card is wider. It documents the analyst, data sources, measurement procedures, sample types, age ranges, comparative-reference sources, and nominal central coverage of the equivalence interval. **Note 6** remains fully visible and explains the accepted source categories for the comparative reference interval. The coverage selector offers 80% and 90%, defaults to 90%, and displays a dynamic **Note 7** explaining the corresponding standard-normal quantile and methodological proposal. The **Assessment groups** and **Compared methods** cards have equal, narrower widths. On intermediate screens, Analysis information occupies the first row while the other two cards remain side by side; on small screens, all three cards are stacked.

## Requirements

- R 4.2 or later;
- packages `shiny`, `kableExtra`, and `xml2`;
- Cairo graphics support, normally included in current R installations.

Install the required packages once:

```r
install.packages(c("shiny", "kableExtra", "xml2"))
```

## Running the application

Open `app.R` in RStudio and click **Run App**. Alternatively, run:

```r
shiny::runApp("path/to/PRIDE-RL-Shiny")
```

## Workflow

1. Complete the analysis-information fields and select the nominal central coverage of the equivalence interval: 80% (Haeckel et al., 2016) or 90% (Dias et al., 2027; LabRI). If no valid value is available, the application uses 90%.
2. Edit the method names and click **Apply method names**. All names may be changed at once.
3. Add the required assessment groups, edit their names in the complete list, and select the active data-entry group.
4. For each group, enter the analyte, unit, sample size, decimal places, comparative limits, and limits estimated by each method.
5. Optionally click **Save Configuration** to store the full setup and entered data in `1_Outputs/saved_params.rds`.
6. Click **Generate Analysis**.
7. Review the tables, rankings, and heatmap. The HTML report and 600 dpi heatmap are saved automatically.
8. Use **Download HTML Report** when a browser download is also required. The heatmap has no download button because the 600 dpi PNG is written automatically.

### Managing methods

- Each method name appears in an editable text field; selecting a method before renaming it is unnecessary.
- **Apply method names** validates and updates all names simultaneously.
- **Add new method** immediately creates another field and corresponding LRL/URL columns in every group.
- To delete a method, select it under **Method to remove** and confirm the removal.
- Renaming a method does not erase entered limits because values remain linked to the method's internal identifier.

### Managing groups

- All registered groups remain visible in a list of editable fields.
- **Apply group names** validates and updates all names simultaneously.
- **Active data-entry group** determines which dataset is displayed in the data-entry grid.
- **Add new group** creates an empty dataset and selects it for data entry.
- To delete a group, select it under **Group to remove** and confirm the removal. All other groups and their data are preserved.

### Entering data

- Data entry uses a purpose-built HTML form rather than a DataTable cell editor.
- The fixed entry columns follow the order **Analyte → Unit → Sample n → Decimal places**, followed by the comparative reference and method limits.
- The `LRL` and `URL` subheaders use the same dark teal background and white text as the main table header, including for methods added dynamically.
- Header groups use the same thin 1 px divider as the remaining table columns, without a thicker white separator before each method.
- Each value remains visible in a wide field, and numeric fields do not show spinner arrows.
- A block of rows and columns copied from Excel, Google Sheets, or a CSV opened in a spreadsheet can be pasted directly into the grid.
- To paste a block, click the first destination cell and press `Ctrl+V`; values are distributed to the right and downward.
- Additional rows are created automatically when the pasted block exceeds the existing rows.
- Values pasted into **Decimal places** are captured through a dedicated flat-data channel, converted to validated integers from 0 to 8, and synchronized with the corresponding drop-down controls. Before saving or generating the analysis, the visible drop-down values are synchronized once more with the server, without requiring a manual change.
- After a multi-cell paste, the horizontal and vertical scroll positions are restored so the visible part of the grid remains anchored.
- Decimal points and decimal commas are accepted. The parser uses the browser locale, numeric structure, and integer-column context to interpret pasted values; formats such as `1,234.56` and `1.234,56` are both supported.
- The **Row** and **Analyte** columns remain visible during horizontal scrolling.
- Use `Tab` or `Enter` to move between fields.
- Each row has a removal button, and **Add analyte** creates a new row at the bottom.
- Changes are saved automatically before calculations are run.

The command buttons are displayed together: blue **Save Configuration**, pale-yellow **Clear Configuration**, green **Generate Analysis**, and purple **Download HTML Report**. Clear Configuration resets the interface and removes `saved_params.rds` after confirmation, but retains previously generated reports and figures.

When the analysis is run, the application writes:

- `1_Outputs/Figures/PRIDe-RL_heatmap_600dpi.png`;
- `1_Outputs/PRIDe-RL_Report.html`.

The generated HTML report is self-contained and uses the same blue visual identity as the Shiny interface. It includes the Base64-embedded PRIDe-RL logo, a responsive lateral navigation menu, the complete analysis-information fields, assessment groups, compared methods, the full data-entry tables, rankings, equivalence-interval calculations, detailed assessments, the heatmap, and the same methodological-reference list presented in the Shiny interface. Each DOI is an explicit link that opens in a new browser tab. The lateral menu uses a CSS-native fallback in addition to JavaScript, 48 px touch targets, safe-area insets, an outside-tap scrim, and adaptive layouts for iPhone, iPad, Android phones, and Android tablets in portrait or landscape orientation. Empty analysis-information fields are reported as **Not provided.** A saved configuration is restored automatically when the application is opened again.

## Outputs

- intermediate parameters from the Haeckel approach: CVE, pCVA, Medln, pSDA, slope, intercept, and pD;
- LRL and URL equivalence intervals;
- inclusion indicator for each estimated limit;
- reference limit deviation index `D_RL = (estimated RL − comparative RL) / pD`;
- direct ranking based on the proportion of limits within the equivalence intervals;
- composite PRIDE-RL ranking by group and globally;
- combined heatmap for all groups;
- self-contained HTML report with responsive lateral navigation, embedded logo, complete input information, styled tables, rankings, and heatmap.

### Result tables

- The four result tables are rendered as HTML with `kableExtra`, without DataTable filters, pagination, or controls.
- Headers use dark blue with white text; the first column uses blue with white text, and all remaining cells use black text.
- Alternating rows, hover highlighting, and horizontal scrolling support reading wide tables.
- The first ranking-table column is named **Group**; internal row indices and names are not displayed.
- Consecutive identical values in **Group** are merged vertically, including each group and the **Global** block, in both the Shiny interface and HTML report.
- Each table includes its own title and methodological note.

#### Composite ranking

- The subtab presents one detailed table for each group and one table for the Global result.
- The two-level header groups **Conformity**, **Proximity**, **Tail behaviour**, and **Severity** over two columns each: one observed value and its normalized `d1`, `d2`, `d3`, or `d4` value.
- `D_RL` expressions use `RL` as a subscript in HTML headers.
- Detailed tables preserve the original method order in **Idx** and display the corresponding **Rank**.
- **PRIDE-RL score** uses a continuous yellow-to-green scale, with higher values shown in greener cells.
- The final **Ordered ranking** table aligns Rank, Method, and PRIDE-RL score for each group and the Global result by position.
- Merged header cells do not display decorative internal lines; only structural table dividers remain.
- In detailed tables, **Rank**, **Method**, **Idx**, **N limits**, and **PRIDE-RL score** span both header levels vertically. **Pos.** behaves the same way in **Ordered ranking**.

### Heatmap structure

- The PNG contains only the scientific figure: matrix, values, descriptive columns, and method names.
- The title and color legend remain outside the figure in both the Shiny interface and report.
- Group, analyte/test, unit, sample size, and RL (`LRL` or `URL`) appear in separate columns in that order, followed by the method columns.
- The group column is merged for each dataset and its label is displayed vertically without repetition, using a black background and bold white text set only slightly larger than the remaining table text.
- Column labels are horizontal.
- Group, Analyte/Test, Unit, n, and RL widths are calculated independently from their longest content.
- All method columns use one standardized width based on the longest method name or formatted value.
- Font size accounts for the number of rows and methods while enforcing a readable minimum.
- Row height is compact and adaptive, gradually decreasing for larger datasets without excessive whitespace.
- Column labels remain close to the upper table border without overlap.
- All heatmap text is black, except for the white labels in the merged Group cells.

## Spreadsheet rules preserved

- when the LRL is zero, 15% of the URL is used only for the logarithmic CVE and median calculations;
- direct inclusion: `|D_RL| ≤ 1`;
- d1 — conformity: `n(|D_RL| ≤ 1) / N`;
- d2 — proximity: `1 / (1 + mean(|D_RL|))`;
- d3 — tail behaviour: `1 / (1 + max(0, P90(|D_RL|) − 1))`;
- d4 — severity: `1 / (1 + 5 × n(|D_RL| > 2) / N)`;
- 90th percentile: inclusive quantile equivalent to Excel's `PERCENTILE.INC`;
- tied rankings: `Rank` remains equal for identical scores (`RANK.EQ`), while `Position` uses progressive tie-breaking based on the original registration order, equivalent to the spreadsheet's progressive `COUNTIF`;
- composite score: equally weighted geometric mean of d1, d2, d3, and d4.

Dimensions d2, d3, and d4 are strictly positive reciprocal transformations. Therefore, none mechanically reduces the composite score to zero when valid data are available.

Sample size `n` identifies the dataset but does not weight the ranking. Each available limit contributes once, as in the original spreadsheet.

Intended for research and laboratory quality assurance; not intended for individual diagnostic decisions.
