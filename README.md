# PRIDe-RL Shiny Application

[![PRIDe-RL Shiny Application](https://img.shields.io/badge/PRIDe--RL%20Shiny%20Application-%230070C0?style=for-the-badge&logoColor=white)](https://github.com/labrgrupo/PRIDe-RL_shiny)

[![License: GPL-3.0](https://img.shields.io/github/license/labrgrupo/PRIDe-RL_shiny.svg)](https://www.gnu.org/licenses/gpl-3.0.en.html)
[![Last commit](https://img.shields.io/github/last-commit/labrgrupo/PRIDe-RL_shiny/main.svg)](https://github.com/labrgrupo/PRIDe-RL_shiny/commits/main)

<img src="logo_PRIDe-RL.png" width="350px" align="right" alt="PRIDe-RL logo" />

The **PRIDe-RL Shiny Application** implements the **Performance Ranking by Index of Deviation of Reference Limits (PRIDe-RL)** framework. It supports the comparative evaluation and ranking of two or more methods that estimate lower and upper reference limits.

The application compares each estimated reference limit with a user-defined **comparative reference limit**, calculates permissible-difference-based **equivalence intervals**, derives the **reference limit deviation index** (`D_RL`), displays the results in a heatmap, and summarizes performance through direct and multidimensional rankings.

This repository contains the **local desktop version**. It runs on the user's computer after R and RStudio are installed and automatically saves the configuration, a 600 dpi heatmap, and a self-contained HTML report in local output folders.

This repository includes four key components:

- **`Install_packages.Rmd`** — checks and installs the R packages required to run PRIDe-RL.
- **`app.R`** — launches the Shiny application and contains the user interface, calculations, tables, rankings, heatmap, methodological documentation, references, and HTML-report generator.
- **`Read_Me/`** — provides a beginner-oriented installation and user guide in Markdown, R Markdown, and PDF formats.
- **`1_Outputs/`** — stores saved configurations, generated HTML reports, and exported figures.

The PRIDe-RL logo and the high-resolution scoring-framework figure used inside the application are embedded directly in `app.R` as Base64 data. The local `logo_PRIDe-RL.png` file is used only to display the project logo on this GitHub page.

> A **cloud-ready** version designed for Posit Connect, Posit Cloud, and institutional Shiny servers is available at [**PRIDE-RL_shiny_connect**](https://github.com/labrgrupo/PRIDE-RL_shiny_connect). See the [comparison table](#local-execution-vs-cloud-ready-version) below.

---

## User interface

The **Data entry** tab organizes the workflow into three main areas: analysis information, assessment groups, and compared methods. The data-entry grid accepts manual entry or direct paste from Excel, Google Sheets, and CSV files opened in spreadsheet software.

After the analysis is generated, the remaining tabs present the detailed results, rankings, heatmap, PRIDe-RL scoring framework, and methodological references.

The interface is responsive and can be used on desktop computers, notebooks, tablets, and mobile devices. The application header displays the PRIDe-RL logo, complete framework name, subtitle, and current version.

---

## Download the PRIDe-RL Shiny Application

<div align="center">

### Click below to download the local application

<a href="https://github.com/labrgrupo/PRIDe-RL_shiny/archive/refs/heads/main.zip">
  <img src="https://img.shields.io/badge/Download%20PRIDe--RL%20Shiny-%23009C3B?style=for-the-badge&logo=github&logoColor=%23009C3B&labelColor=%23FFDF00" alt="Download the PRIDe-RL Shiny Application" style="height:50px;" />
</a>

</div>

After downloading, extract the complete ZIP file before opening the application. Do not run PRIDe-RL from inside the compressed file.

---

## Live demonstration

A public demonstration of the cloud-ready version is available on Posit Connect Cloud:

<div align="center">

<a href="https://labrgroup-pride-rl.share.connect.posit.cloud/" target="_blank">
  <img src="https://img.shields.io/badge/Launch%20PRIDe--RL%20Demo-%23009C3B?style=for-the-badge&logo=google-chrome&logoColor=%23009C3B&labelColor=%23FFDF00" alt="Launch the PRIDe-RL live demonstration" style="height:50px;" />
</a>

</div>

The public deployment is intended for demonstration and training. Do not enter confidential, identifiable, or institutionally restricted information in the public version.

---

## Local execution vs. cloud-ready version

The PRIDe-RL tool is distributed in two complementary implementations that use the same analytical framework but differ in how files and user sessions are managed:

- **PRIDe-RL Shiny Application (this repository)** — runs locally after R and RStudio are installed. It can retain the complete configuration and automatically writes the HTML report and 600 dpi heatmap to `1_Outputs/`.
- **[PRIDE-RL_shiny_connect](https://github.com/labrgrupo/PRIDE-RL_shiny_connect)** — cloud-ready implementation for Posit Connect, Posit Cloud, and institutional Shiny servers. It operates in session-based mode without persistently writing user outputs to the server directory. The HTML report can be downloaded manually.

| Feature | PRIDe-RL Shiny Application | PRIDE-RL_shiny_connect |
|---|:---:|:---:|
| Primary use | Local desktop analysis | Web or institutional deployment |
| Runs on the user's computer | Yes | Optional |
| Requires R and RStudio for the end user | Yes | No, when already deployed |
| Designed for Posit Connect or Posit Cloud | No | Yes |
| Saves the complete configuration locally | Yes | No persistent server copy |
| Automatically saves the 600 dpi heatmap | Yes | No persistent server copy |
| Automatically saves the HTML report | Yes | No persistent server copy |
| Allows manual HTML-report download | Yes | Yes |
| Uses session-specific temporary files | No | Yes |
| Suitable for multi-user browser access | No | Yes |

The local version is recommended for individual analytical work, reproducible desktop workflows, and automatic retention of outputs. The cloud-ready version is intended for demonstrations, teaching, institutional deployment, and multi-user browser access.

---

## The PRIDe-RL framework

PRIDe-RL is a multidimensional framework for comparing methods that estimate reference limits. It does not estimate a reference interval from raw patient results. Instead, it evaluates how closely the reference limits produced by different methods agree with a specified comparative reference.

The framework follows five main stages:

1. **Comparative reference definition** — the user provides a comparative lower reference limit (`LRL`) and upper reference limit (`URL`) for each analyte or test.
2. **Equivalence-interval calculation** — a permissible difference is calculated separately around the comparative LRL and URL using the permissible analytical uncertainty associated with each reference limit.
3. **Deviation normalization** — each estimated reference limit is compared with its corresponding comparative limit using the `D_RL` index.
4. **Visual assessment** — the heatmap displays the direction and magnitude of the deviations for every analyte, reference limit, and method.
5. **Quantitative ranking** — direct inclusion and four complementary dimensions describe and summarize method performance.

### Equivalence intervals

The equivalence interval defines the range within which the difference between an estimated reference limit and its comparative reference limit is considered not clinically relevant under the method's permissible-uncertainty criterion.

The application offers two nominal central coverage options:

- **80%**, corresponding to the original proposal of Haeckel et al. (2016);
- **90%**, corresponding to the criterion proposed by Dias et al. for the LabRI method.

The interval is calculated separately for the LRL and URL of every analyte.

### Reference limit deviation index

The reference limit deviation index is defined as:

```text
D_RL = (estimated RL - comparative RL) / permissible difference
```

Its interpretation is straightforward:

- `D_RL = 0` indicates agreement with the comparative reference limit;
- `|D_RL| <= 1` indicates inclusion within the equivalence interval;
- `|D_RL| > 1` indicates that the estimate lies outside the equivalence interval;
- the sign indicates whether the estimated limit is below or above the comparative limit.

Because the difference is divided by the permissible difference for the corresponding reference limit, the index provides a common normalized scale for comparing analytes with different units and numerical ranges.

### Performance dimensions

PRIDe-RL evaluates the distribution of the `D_RL` indices through four complementary dimensions:

- **d1 — Conformity:** proportion of evaluated limits contained within their equivalence intervals;
- **d2 — Proximity:** overall closeness of the estimated limits to their comparative limits;
- **d3 — Tail behaviour:** behaviour of the upper tail of the absolute deviation distribution;
- **d4 — Severity:** frequency of deviations greater than twice the permissible difference.

The **PRIDe-RL score** is the equally weighted geometric mean of d1, d2, d3, and d4. It integrates conformity, proximity, tail behaviour, and severity into a single measure ranging from 0 to 1. Higher values indicate better overall agreement with the comparative reference limits.

This multidimensional approach translates the visual pattern of the heatmap into a quantitative summary. Methods with predominantly greener cells tend to perform better, whereas methods with more orange and red cells tend to receive lower scores.

The complete equations, variable definitions, methodological notes, and bibliography are available inside the application under **PRIDe-RL scoring framework** and **References**.

---

## System requirements

Before running the local application, install:

1. **R 4.2 or later** — [https://cran.r-project.org/](https://cran.r-project.org/)
2. **RStudio Desktop** — [https://posit.co/downloads/](https://posit.co/downloads/)
3. The required R packages:
   - `shiny`
   - `kableExtra`
   - `xml2`

An internet connection is required for the initial installation. Analyses are subsequently performed on the local computer.

---

## Installation

### 1. Install R

1. Open the [official R website](https://cran.r-project.org/).
2. Select the installer for Windows, macOS, or Linux.
3. Download and install the current version of R.
4. Keep the default installation options unless your institution has specific requirements.

R must be installed before RStudio.

### 2. Install RStudio Desktop

1. Open the [official Posit download page](https://posit.co/downloads/).
2. Download the free **RStudio Desktop** installer for your operating system.
3. Install RStudio using the default options.

RStudio is the interface used to open and run PRIDe-RL; it does not replace R.

### 3. Extract the PRIDe-RL package

1. Download the repository as a ZIP file.
2. Extract the complete ZIP file to a normal folder, such as `Documents/PRIDe-RL-Shiny`.
3. Keep the original folder structure unchanged.
4. Confirm that `app.R`, `Install_packages.Rmd`, `Read_Me/`, and `1_Outputs/` are present.

Choose a folder where you have permission to create and modify files.

### 4. Install the required R packages

This procedure normally needs to be completed only once for each R installation.

1. Open **RStudio Desktop**.
2. Select **File > Open File**.
3. Open `Install_packages.Rmd` from the extracted PRIDe-RL folder.
4. Find the code block headed by `CLICK RUN CURRENT CHUNK`.
5. Click anywhere inside that code block.
6. Click **Run Current Chunk**, or click the small green triangle at the upper-right corner of the block.
7. Wait until installation and verification are complete.
8. Confirm that `shiny`, `kableExtra`, and `xml2` are reported as `TRUE`.

Packages already installed are not installed again. If a package is reported as `FALSE`, check the RStudio Console, confirm the internet connection, and run the chunk again.

---

## Start the application

After the required packages are available:

1. Close `Install_packages.Rmd`.
2. In RStudio, select **File > Open File**.
3. Open `app.R` from the same PRIDe-RL folder.
4. Click **Run App** at the upper-right corner of the script editor.
5. Wait for PRIDe-RL to open in the RStudio Viewer or your default web browser.

Keep RStudio open while using the application. If the browser does not open automatically, copy the local address displayed in the RStudio Console, normally beginning with `http://127.0.0.1:`.

Alternatively, the application can be started from R with:

```r
shiny::runApp("path/to/PRIDe-RL-Shiny")
```

---

## User workflow

### 1. Complete the analysis information

In the **Data entry** tab, document the responsible specialist, data source, measurement procedure and analytical method, sample type, age range, and source of the comparative reference.

Fields that are not completed are reported as **Not provided.** in the generated HTML report.

### 2. Select the nominal central coverage

Select **80%** or **90%** under **Nominal central coverage of the equivalence interval**. The default is 90%. The explanatory note changes automatically according to the selected option.

### 3. Configure assessment groups

- Add the required groups.
- Edit all group names directly.
- Click **Apply group names**.
- Select the group whose data will be entered under **Active data-entry group**.

Each group retains its own analytes and method estimates.

### 4. Configure the compared methods

- Rename the existing methods directly.
- Click **Apply method names**.
- Use **Add new method** when additional methods are required.
- Use **Method to remove** to delete a method.

Renaming a method does not erase its entered values because the limits remain linked to an internal method identifier.

### 5. Enter the analyte data

Enter one row per analyte or test. The required columns are:

- analyte or test name;
- unit;
- sample size (`n`);
- decimal places;
- comparative LRL and URL;
- LRL and URL estimated by each method.

A rectangular block copied from Excel, Google Sheets, or a CSV file opened in a spreadsheet can be pasted directly into the grid. Click the first destination cell and press `Ctrl+V`.

Decimal points, decimal commas, and common local thousands separators are recognized. Values pasted into **Decimal places** are synchronized with the corresponding drop-down lists and validated as integers from 0 to 8.

### 6. Save the configuration

Click **Save Configuration** to save the complete setup and entered values in:

```text
1_Outputs/saved_params.rds
```

The saved configuration is restored when the application is opened again.

### 7. Generate the analysis

Click **Generate Analysis**. If validation messages are displayed, correct the indicated group and row and run the analysis again.

After calculation, review:

- **Results** — equivalence intervals, detailed assessments, direct ranking, and composite ranking;
- **Heatmap** — visual overview of the `D_RL` indices;
- **PRIDe-RL scoring framework** — workflow figure, scoring equations, and variable definitions;
- **References** — complete methodological bibliography with clickable DOI links.

If any input is changed, click **Generate Analysis** again to update the results and exported files.

---

## Outputs

The local application creates or updates the following files. `saved_params.rds` is created when **Save Configuration** is selected; the report and heatmap are generated when the analysis is run.

```text
1_Outputs/
├── saved_params.rds
├── PRIDe-RL_Report.html
└── Figures/
    └── PRIDe-RL_heatmap_600dpi.png
```

### Analytical outputs

- intermediate permissible-uncertainty parameters;
- LRL and URL equivalence intervals;
- inclusion status for each estimated reference limit;
- `D_RL` index for each limit and method;
- direct ranking based on inclusion within the equivalence intervals;
- d1, d2, d3, and d4 performance dimensions;
- PRIDe-RL score and composite ranking by group and globally;
- combined heatmap for all assessment groups.

### Self-contained HTML report

The generated report includes:

- the PRIDe-RL logo and application version;
- all supplied analysis information;
- group and method configurations;
- complete input-data tables;
- equivalence intervals and detailed assessments;
- direct and composite rankings;
- the PRIDe-RL heatmap;
- the scoring-framework figure and definitions;
- the complete bibliography with clickable DOI links;
- responsive lateral navigation for desktop computers, iPhones, iPads, Android phones, and Android tablets.

The HTML report can also be saved through **Download HTML Report** after the analysis has been generated.

---

## Heatmap interpretation

The heatmap uses a continuous green-yellow-red scale:

- values near `D_RL = 0` are greener and indicate greater agreement;
- values near `|D_RL| = 1` approach the equivalence boundary;
- orange and red cells indicate larger deviations outside the equivalence interval;
- the sign of `D_RL` indicates the direction of the deviation.

The merged **Group** labels use an adaptive font size. The label becomes moderately larger when it spans more rows while remaining constrained to fit the available cell dimensions.

Sample size `n` identifies each dataset but does not weight the rankings. Each available reference limit contributes once to the corresponding assessment scope.

---

## Stop the application

When the analysis is complete:

1. Return to RStudio.
2. Click the red **Stop** button in the Console area, or press `Esc`.
3. Confirm that the R Console prompt (`>`) is visible again.

Closing only the browser tab may leave the Shiny process running in RStudio.

---

## Troubleshooting

### A required package is missing

Open `Install_packages.Rmd` and run the installation chunk again. Restart RStudio if the package remains unavailable.

### The Run App button is not visible

Confirm that `app.R` is the active file in the RStudio editor.

### The application does not open in the browser

Check the RStudio Console for a local address beginning with `http://127.0.0.1:` and open it manually.

### Reports or figures are not created

Confirm that the ZIP file was fully extracted and that the selected folder allows files to be created. Do not run the application from inside the ZIP file or from a protected system directory.

### Pasted decimal-place values are reported as missing

Use the current version of the application, paste the complete block again, and generate the analysis. The decimal-place drop-down lists are synchronized automatically after spreadsheet paste operations.

### The application reports invalid data

Read every validation message and correct the corresponding group and row. Check sample sizes, decimal places, comparative limits, and all estimated limits.

---

## Important use statement

PRIDe-RL is intended for research, method comparison, and laboratory quality-assurance activities. Results must be interpreted by qualified professionals together with the analytical, biological, and clinical context.

The application is not intended to make individual diagnostic decisions or replace professional judgment.

---

## Contact

**Suggestions and bug reports:**  
[https://github.com/labrgrupo/PRIDe-RL_shiny/issues](https://github.com/labrgrupo/PRIDe-RL_shiny/issues)

**Email:**  
[alancdias@hotmail.com](mailto:alancdias@hotmail.com) · [labrgrupo@gmail.com](mailto:labrgrupo@gmail.com)

**Lab R Group website:**  
[https://grupolabr.com/](https://grupolabr.com/)

---

## License

Distributed under the **GNU General Public License v3.0 (GPL-3.0)**.
