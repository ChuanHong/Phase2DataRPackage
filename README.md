# Phase2DataRPackage
This repository contains R utility functions for Phase 2 Data Pre-Processing for the 4CE Consortium.

# Installation

To install this package in R:

``` R
devtools::install_github("https://github.com/ChuanHong/Phase2DataRPackage", subdir="FourCePhase2Data", upgrade=FALSE)
```

# 4CE Phase 2 Data Pre-Processing Overview

The Phase 2 Data Pre-Processing is recommended for all sites before running the 4CE consortium Phase 2 project. The Phase 2 Data Pre-Processing consists of two parts: quality control and data cleaning. 


## Quality Control for Phase 2 Data

The R library (`FourCePhase2Data`) in this repository contains functions that conduct quality control for the Phase 2 Data: 
1. **QC for Phase 2 patient-level data**. We will check if the summary statistics obtained from Phase 2 patient-level data is consistent with the aggreated results generated for Phase1.1.
2. **QC for Phase 2 summary statistics**. We will check the following critera: 
+ Demographics:  
+ + if there is missing demographic groups (e.g., missing age group); 
+ + if there is negative patient numbres or counts; 
+ + if there is number of all patients less than the number of severe patients. 
+ Medications and Diagnoses:
+ + if there is any code not belong to the list of medclass or ICD10; 
+ + if there is negative patient numbers or counts; 
+ + if the number of all patients is greater than the number of severe patients.
+ Labs: 
+ + if there is any labs not belong to the list of loinc codes;
+ + if there is negative patients nubmers; 
+ + if there is negative measures in the original scale.
+ Cross over comparison: 
+ + if the number of patients in DailyCounts, ClincalCourse, and Demographic are consistent; 
+ + if the number of severe patients in DailyCounts, ClinialCourse, and Demographic are consistent;
+ + if in any of Medications, Diagnoses or Labs, the number of patients with the code is greater than number of all patients. 
+ Lab units: check if the lab measures are consistently outside the confidence range.
3. **generate QC report**
A QC report will be generated in word format. 


## Data Cleaning for Phase 2 Data

The R library (`FourCePhase2Data`) in this repository also contains functions for data clean, which generates data in specific formats to fit into different analyses:  
1. **Longitudinal data for Labs**: days since admission 
2. **Longitudinal data for Medications**: before and since admission 
3. **Longitudinal data for Diagnoses**: before and since admission 
4. **Baseline covariates**: demographic variables, lab measures at day 0, medications and diagnoses before admission. 
5. **Time varying covariates**: to be added
6. **Event time data**: for each event outcome (severe, deceased, and severe AND deceased), we derive observed event time, and the indicator of obsering the event. 


To get started, run the Docker container (https://github.com/covidclinical/Phase2.0_Docker_Analysis).


The latest versions of the 4CE Docker container come pre-configured with this R library installed. To ensure that you have the latest version of this software, however, you should run the following in your R session in the container before proceeding:

``` R
devtools::install_github("https://github.com/covidclinical/Phase2UtilitiesRPackage", subdir="FourCePhase2Utilities", upgrade=FALSE)
```

In order to perform the following steps you **must** have been granted permission to create repositories under the [covidclinical GitHub organization](https://github.com/covidclinical).  Only members of the 4CE consortium will be granted this permission, and they should request it through the [phase-2 4CE slack channel](https://covidclinical.slack.com/archives/C012UTRHJCR).

In the rest of this example, we will create a new project called "MyAnalysis". This example is for illustrative purposes only.
You should replace "MyAnalysis" with the name of the project that you are creating before running the code.

After having installed the `FourCePhase2Utilities` R library (see above), in your command-line R session
you can begin the process of creating the repositories for the "MyAnalysis" project with the following command
(you will be prompted for your GitHub username and password **6 times** as the
repositories are created and pre-populated):

```
FourCePhase2Utilities::createProject("MyAnalysis")
```

This `createProject(...)` function call will generate all of the repositories required to support a 4CE Phase 2 project.  By default,
the local copies of these repositories are created under `/RDevelopment/` in the container. You can provide an alternative location by specifying the `workingDirectory` argument to `createProject(...)`.

You would now modify `runAnalysis()`, `validateAnalysis()`, `submitAnalysis()` inside of the
`Phase2MyAnalysisRPackage` repository.  Note that the R package lives in a subdirectory
of the repository named `FourCePhase2[PROJECT_NAME]` (`FourCePhase2MyAnalysis` in our example). The `README.md` for your package comes pre-populated with example code to install your package in R from this subdirectory.  You can add as much additional supporting code as you would like
to your package in additional files, or in the files for these three functions.  For more information on developing R packages in general, see http://r-pkgs.had.co.nz

The R package that was generated by `createProject(...)` contains two functions, `getSiteDataRepositoryUrl()` and `getAggregateCountryDataRepositoryUrl()` that return respectively the GitHub URLs of the repositories for the site and country level data for your project.  These may be useful when writing validation and submission code.

To help other consortium members understand and run your code, please include well-thought-out comments and documentation.  Please see http://r-pkgs.had.co.nz/man.html for information on documenting packages and functions using Roxygen.

Once you are done developing your code, generate documentation using the devtools (Roxygen) package,
with the R working directory set to the package's directory. You don't need to run this in the same R
session as above, in fact, the remainder of this code works fine in RStudio.

Note that we are running this **inside the R package directory within the git repository**:

```
setwd("/RDevelopment/Phase2MyAnalysisRPackage/FourCePhase2MyAnalysis")
devtools::document()
```

The R package for your project is now ready to push to GitHub to become available to run at the 4CE sites, and you can do so using your
tool of choice for working with git.  Since the local copies are stored on the docker host's
storage (if you ran the container with a `-v` option specifying a host file system location, and used that location when calling `createProject(...)`), this can either be done inside the container, or in the host environemt.  If working
in the host environment, you would need to adjust file locations appropriately.


If all of this has worked correctly, you should be able to see the following files in the output directory:

<pre>https://github.com/covidclinical/Phase2[PROJECT_NAME]]RPackage</pre>

(https://github.com/covidclinical/Phase2MyAnalysisRPackage in our example)

and see the modifications that you pushed.  You can install your package using the code displayed on the README (main) page for the repository, e.g.:
