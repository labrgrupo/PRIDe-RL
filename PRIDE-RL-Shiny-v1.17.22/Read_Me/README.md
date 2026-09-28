# PRIDe-RL - Installation and User Guide

## 1. About the application

**PRIDe-RL** stands for **Performance Ranking by Index of Deviation of Reference Limits**. It is an R Shiny application for comparing reference limits estimated by two or more methods against comparative reference limits. The application calculates equivalence intervals, reference-limit deviation indices, performance dimensions, rankings, and a PRIDe Score, and it generates a heatmap and a self-contained HTML report.

No programming experience is required to use the application. Follow the steps below in the stated order.

## 2. What must be installed

You need:

1. **R**, the statistical computing environment.
2. **RStudio Desktop**, the program used to open and run the application.
3. The three R packages required by PRIDe-RL: `shiny`, `kableExtra`, and `xml2`.

An internet connection is required during the initial installation of R, RStudio, and the R packages. After installation, the analyses are performed locally on your computer.

## 3. Install R

1. Open the official R website: [https://cran.r-project.org/](https://cran.r-project.org/).
2. Select the link for your operating system: Windows, macOS, or Linux.
3. Download the current release of R.
4. Run the installer.
5. Keep the default installation options unless your institution has specific requirements.

R must be installed before RStudio.

## 4. Install RStudio Desktop

1. Open the official Posit download page: [https://posit.co/downloads](https://posit.co/downloads).
2. Locate **RStudio Desktop**.
3. Download the free version for your operating system.
4. Run the installer and keep the default options.
5. Start RStudio once the installation is complete.

RStudio is the interface used to run PRIDe-RL; it does not replace R.

## 5. Prepare the PRIDe-RL folder

1. Download the PRIDe-RL ZIP package.
2. Extract the complete ZIP file to a normal folder on your computer.
3. Do not run the application from inside the compressed ZIP file.
4. Keep the complete folder structure together. The main items are:

   - `app.R`: the PRIDe-RL application.
   - `Install_packages.Rmd`: the package installer.
   - `Read_Me`: this user guide in Markdown, R Markdown, and PDF formats.
   - `1_Outputs`: the folder where saved configurations, reports, and figures are written.

Choose a location where you have permission to create and modify files, such as your Documents folder.

## 6. Install the required R packages

This procedure normally needs to be completed only once on each computer or R installation.

1. Open **RStudio Desktop**.
2. In RStudio, select **File > Open File**.
3. Open `Install_packages.Rmd` from the extracted PRIDe-RL folder.
4. If RStudio asks to install components required to work with R Markdown files, accept the installation.
5. Find the code block headed by `CLICK RUN CURRENT CHUNK`.
6. Click anywhere inside that code block.
7. Click **Run Current Chunk**. Depending on the RStudio version, this command is available from the **Run** menu above the script editor or from the small green triangle at the upper-right corner of the code block.
8. Wait until the installation finishes. Do not close RStudio while packages are being downloaded and installed.
9. Review the package-status table. The following packages should all display `TRUE`:

   - `shiny`
   - `kableExtra`
   - `xml2`

Packages already available on the computer are not reinstalled. If any package displays `FALSE`, review the messages in the RStudio Console, confirm that the computer is connected to the internet, and run the same chunk again.

## 7. Start the PRIDe-RL application

After all three packages display `TRUE`:

1. Close `Install_packages.Rmd`. Saving changes to this installer is not necessary.
2. Select **File > Open File** in RStudio.
3. Open `app.R` from the same PRIDe-RL folder.
4. Wait until the file is fully displayed in the script editor.
5. Click **Run App** at the upper-right corner of the editor.
6. Wait for PRIDe-RL to open in the RStudio Viewer or in your default web browser.

Keep RStudio open while using the application. If the browser does not open automatically, look in the RStudio Console for an address similar to `http://127.0.0.1:xxxx` and open that address in your browser.

## 8. Basic workflow in PRIDe-RL

### 8.1. Enter the assessment information

In the **Data entry** tab:

1. Complete the available fields in **Analysis information**.
2. Review or create the required **Assessment groups**.
3. Review, rename, add, or remove the **Compared methods**.
4. Select the **Nominal central coverage of the equivalence interval**. The available options are 80% and 90%; 90% is the default.

Information that is not supplied will be identified as not provided in the generated report.

### 8.2. Enter the analyte data

1. Enter one row per analyte in each assessment group.
2. Provide the analyte, unit, sample size, decimal places, comparative LRL and URL, and the LRL and URL estimated by each method.
3. You may copy a rectangular block directly from Excel, Google Sheets, or another spreadsheet and paste it into the first destination cell.
4. Decimal points and decimal commas are accepted. Common local thousands separators are also recognized.
5. The **Decimal places** column remains a drop-down list, including after spreadsheet data are pasted.

### 8.3. Generate the analysis

1. Review the entered information.
2. Click **Generate Analysis**.
3. If validation messages appear, correct the indicated rows and click **Generate Analysis** again.
4. Review the available tabs:

   - **Results**: equivalence intervals, detailed indices, and rankings.
   - **Heatmap**: graphical overview of the reference-limit deviation indices.
   - **PRIDe-RL scoring framework**: methodological workflow and scoring formulas.
   - **References**: methodological bibliography with clickable DOI links.

If any input is changed after the calculation, run **Generate Analysis** again to update the results and exported files.

## 9. Save the configuration and download the report

- Click **Save Configuration** to retain the current application settings and data in `1_Outputs/saved_params.rds`.
- Use the HTML-report download control after generating the analysis. The generated report includes the entered information, results, heatmap, methodological content, references, PRIDe-RL logo, and responsive navigation.
- The application also writes the following files:

  - `1_Outputs/PRIDe-RL_Report.html`
  - `1_Outputs/Figures/PRIDe-RL_heatmap_600dpi.png`

The HTML report is self-contained and can be opened in a standard web browser. Its responsive navigation works on desktop computers, iPhones, iPads, Android phones, and Android tablets.

## 10. Stop the application

When you finish:

1. Return to RStudio.
2. Click the red **Stop** button in the Console area, or press the Escape key.
3. Confirm that the RStudio Console prompt (`>`) is visible again.

Closing only the browser tab may leave the R Shiny process running in RStudio.

## 11. Troubleshooting

### A package is reported as missing

Open `Install_packages.Rmd` and run the installation chunk again. Restart RStudio before retrying if the package remains unavailable.

### A package cannot be downloaded

Check the internet connection, institutional proxy, firewall, and installation permissions. On a managed institutional computer, assistance from the local support team may be required.

### The **Run App** button is not visible

Confirm that `app.R`, not the installer or README, is the active file in the RStudio editor.

### The application does not open in the browser

Check the RStudio Console for a local address beginning with `http://127.0.0.1:` and open it manually. Do not close RStudio.

### Reports or figures are not created

Confirm that the ZIP package was fully extracted and that you have permission to write files in the PRIDe-RL folder. Avoid running the application from a protected system directory or directly from the ZIP file.

### The application reports invalid data

Read every validation message, correct the corresponding group and row, and run the analysis again. Pay particular attention to missing decimal-place selections, reference limits, sample sizes, and estimated limits.

## 12. Important recommendations

- Keep an unchanged copy of the original ZIP package.
- Do not edit `app.R` unless you are intentionally modifying the application source code.
- Keep the entire folder structure together.
- Save a configuration before closing the application when you want to continue the same assessment later.
- Review all entered values before interpreting or distributing results.

## 13. Project link

Source code and project information are available at:

[https://github.com/labrgrupo/PRIDe-RL](https://github.com/labrgrupo/PRIDe-RL)

