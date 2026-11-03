# Persistent Homology, Persistence Diagrams, Persistence Landscapes and Betti Curves Using 42 Biomarkers

## Overview

This repository contains a Python implementation of Topological Data Analysis (TDA) for analysing clinical cancer biomarker data using **Persistent Homology**.

The framework investigates the topological characteristics of four clinical cohorts:

- Normal (Healthy)
- Bladder Cancer
- Ovarian Cancer
- Pancreatic Cancer

The analysis uses 42 biomarkers to construct correlation-based dissimilarity matrices, followed by Vietoris–Rips filtration and persistent homology computation. The resulting topological information is used to generate persistence diagrams, persistence landscapes, and Betti curves.

## Objectives

The primary objectives of this implementation are:

1. To investigate the topological structure of clinical biomarker data across different cohorts.
2. To construct Vietoris–Rips complexes using biomarker correlation-based dissimilarity matrices.
3. To calculate persistent homology in dimensions 0 and 1.
4. To visualize connected components and one-dimensional loops through persistence diagrams.
5. To calculate and visualize H0 and H1 persistence landscapes.
6. To construct and compare Betti-0 and Betti-1 curves across the four cohorts.
7. To generate numerical summaries and export topological features for further statistical analysis.

## Methodology

### 1. Data Loading and Preprocessing

The input clinical dataset is provided in an Excel file. The analysis identifies 42 numerical biomarkers while excluding metadata columns such as patient ID, cohort, age, sex, and clinical staging information.

The dataset is divided into four cohorts:

- Normal
- Bladder
- Ovarian
- Pancreatic

Cohort-wise patient counts and biomarker data completeness are printed during execution.

### 2. Correlation Matrix Construction

For each cohort, a Pearson correlation matrix is calculated using the 42 biomarkers.

The correlation matrix is represented as:

\[
C=(c_{ij})_{42\times42}
\]

where \(c_{ij}\) denotes the Pearson correlation coefficient between biomarkers \(i\) and \(j\).

### 3. Dissimilarity Matrix Construction

The correlation matrix is transformed into a dissimilarity matrix using:

\[
D_{ij}=1-|c_{ij}|
\]

The resulting matrix is symmetric, with zero diagonal entries.

### 4. Vietoris–Rips Filtration

A Vietoris–Rips complex is constructed using the dissimilarity matrix.

The maximum edge length is fixed at 1.0, and the maximum simplex dimension is set to 2.

### 5. Persistent Homology

Persistent homology is calculated using the GUDHI library with coefficient field 2.

Two homological dimensions are investigated:

- **H0:** Represents connected components.
- **H1:** Represents one-dimensional loops or holes.

Birth and death values are extracted for the corresponding topological features.

### 6. Persistence Diagrams

Persistence diagrams are generated separately for all four cohorts.

- H0 features are represented by hollow circular markers.
- H1 features are represented by filled triangular markers.
- The diagonal line represents equal birth and death values.
- Essential persistence intervals are counted and annotated.

### 7. Persistence Landscapes

Persistence landscapes are calculated separately for H0 and H1.

For a finite persistence interval \([b,d]\), the corresponding tent function is:

\[
f(t)=\max(0,\min(t-b,d-t))
\]

The five largest tent-function values at each filtration parameter are used to construct the first five landscape layers:

\[
\lambda_1(t),\lambda_2(t),\ldots,\lambda_5(t)
\]

Separate H0 and H1 landscape figures are generated for every cohort.

### 8. Betti Curves

Betti curves are calculated for H0 and H1 across 501 filtration radii between 0 and 1.

The Betti numbers represent the number of topological features alive at each filtration radius.

Combined Betti-0 and Betti-1 curves are plotted to compare the four clinical cohorts.

## Requirements

The following Python libraries are required:

- Python 3.9 or later
- NumPy
- Pandas
- Matplotlib
- GUDHI
- Openpyxl

Install the required packages using:

```bash
pip install numpy pandas matplotlib gudhi openpyxl
```

## Input Dataset

The input dataset should be provided as an Excel file.

**Expected input filename:**

```text
cancer clinical data.xlsx
```

**Expected worksheet:**

```text
Normal and Cancer
```

The dataset should contain:

- Patient identification information.
- Cohort classification.
- 42 numerical biomarker measurements.

The cohort labels must correspond to:

```text
Normal
Bladder
Ovarian
Pancreatic
```

## Configuration

Before executing the script, specify the input file and output directory in the configuration section:

```python
INPUT_FILE = r"D:\Clinical TDA\Data Analysis\cancer clinical data.xlsx"

SHEET_NAME = "Normal and Cancer"

OUTPUT_DIR = r"D:\Clinical TDA\Data Analysis"
```

Modify these paths according to the location of your dataset and the desired output directory.

The principal analysis parameters are:

| Parameter | Value |
|---|---:|
| Number of biomarkers | 42 |
| Number of cohorts | 4 |
| Maximum edge length | 1.0 |
| Maximum homology dimension | 2 |
| Homology coefficient field | 2 |
| Number of filtration radii | 501 |
| Number of landscape layers | 5 |
| Figure resolution | 600 DPI |

## Execution

After installing the required libraries and configuring the input and output paths, execute the Python script:

```bash
python Persistent_Homology_Analysis.py
```

The program automatically performs the complete analysis for all four cohorts.

During execution, it prints dataset information, cohort-wise patient counts, biomarker completeness, persistence interval summaries, landscape information, and Betti number summaries.

All generated figures are displayed and saved in the specified output directory.

## Generated Outputs

### 1. Persistence Diagrams

Four persistence diagrams are generated:

```text
Persistence_Diagram_Normal.png
Persistence_Diagram_Bladder.png
Persistence_Diagram_Ovarian.png
Persistence_Diagram_Pancreatic.png
```

### 2. Persistence Landscapes

Separate H0 and H1 persistence landscape figures are generated for each cohort.

```text
Persistence_Landscape_H0_Normal.png
Persistence_Landscape_H1_Normal.png

Persistence_Landscape_H0_Bladder.png
Persistence_Landscape_H1_Bladder.png

Persistence_Landscape_H0_Ovarian.png
Persistence_Landscape_H1_Ovarian.png

Persistence_Landscape_H0_Pancreatic.png
Persistence_Landscape_H1_Pancreatic.png
```

### 3. Combined Betti Curves

A combined figure containing Betti-0 and Betti-1 curves for all four cohorts is generated:

```text
Combined_Betti_Curves_All_Cohorts.png
```

### 4. Numerical Excel Outputs

The program also generates the following Excel files:

| Filename | Description |
|---|---|
| `Persistent_Homology_Summary_All_Cohorts.xlsx` | Cohort-wise summary of persistent homology results. |
| `Betti_Curve_Values_All_Cohorts.xlsx` | Numerical Betti-0 and Betti-1 values across filtration radii. |
| `Persistence_Landscape_Values_All_Cohorts.xlsx` | Numerical values of the first five H0 and H1 landscape layers for each cohort. |
| `Persistence_Intervals_All_Cohorts.xlsx` | Birth, death, and persistence values of topological features. |
| `Correlation_and_Dissimilarity_Matrices.xlsx` | Cohort-specific correlation and dissimilarity matrices. |

## Visualization Quality

All figures are generated using publication-oriented plotting settings:

- 600 DPI resolution.
- White background.
- Large and readable axis labels and titles.
- Clearly distinguishable line styles.
- Larger persistence markers.
- Proper legends and annotations.
- Separate H0 and H1 persistence landscape figures.

The generated figures can be used for further scientific analysis and manuscript preparation.

## Software and Libraries

This implementation uses the following scientific Python libraries:

- **GUDHI:** Construction of simplicial complexes and persistent homology computation.
- **NumPy:** Numerical calculations and matrix operations.
- **Pandas:** Clinical dataset processing and Excel file handling.
- **Matplotlib:** Visualization of persistence diagrams, landscapes, and Betti curves.
- **Openpyxl:** Excel output generation.

## Notes

1. The analysis uses all 42 identified biomarkers.
2. Each cohort is analysed independently.
3. The correlation-based dissimilarity is defined as \(D_{ij}=1-|c_{ij}|\).
4. The persistence diagrams and landscapes consider H0 and H1 features.
5. Infinite persistence intervals are excluded from finite persistence landscape calculations.
6. Betti curves include infinite persistence intervals.
7. All generated figures and Excel files are saved in the specified output directory.

## Acknowledgements

This implementation utilizes the GUDHI library for computational topology and persistent homology analysis.

**Research Area:** Topological Data Analysis, Computational Biology, Cancer Biomarker Analysis, Persistent Homology, and Mathematical Biology.
