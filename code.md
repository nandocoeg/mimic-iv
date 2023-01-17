---
description: Halaman ini akan menjelaskan code
---

# Code

The MIMIC Code Repository is intended to be a central hub for sharing, refining, and reusing code used for analysis of the [MIMIC critical care database](https://mimic.mit.edu/). To find out more about MIMIC, please see: [https://mimic.mit.edu](https://mimic.mit.edu/). Source code for the website is in the [mimic-website GitHub repository](https://github.com/MIT-LCP/mimic-website/).

You can read more about the code repository in the following open access paper: [The MIMIC Code Repository: enabling reproducibility in critical care research](https://doi.org/10.1093/jamia/ocx084).

## Cloud Access

The various MIMIC databases are available on Google Cloud Platform (GCP) and Amazon Web Services (AWS). To access the data on the cloud, simply add the relevant cloud identifier to your PhysioNet profile. Then request access to the dataset for the particular cloud platform via the PhysioNet project page. Further instructions are available on [the MIMIC website](https://mimic.mit.edu/iv/access/cloud/).

### Navigating repository

* [MIMIC-III](https://physionet.org/content/mimiciii/) - critical care data for patients admitted to ICUs at the BIDMC between 2001 - 2012
* [MIMIC-IV](https://physionet.org/content/mimiciv/) - hospital and critical care data for patients admitted to the ED or ICU between 2008 - 2019
* [MIMIC-IV-ED](https://physionet.org/content/mimic-iv-ed/) - emergency department data for individuals attending the ED between 2011 - 2019
* MIMIC-IV Waveforms (TBD) - this dataset has yet to be published.
* [MIMIC-CXR](https://physionet.org/content/mimic-cxr/) - chest x-ray imaging and deidentified free-text radiology reports for patients admitted to the ED from 2012 - 2016

This repository contains code for five databases on PhysioNet:

Each subfolder has a README with further detail regarding its content.

* [mimic-iii](https://github.com/MIT-LCP/mimic-code/blob/main/mimic-iii) - build scripts for MIMIC-III, derived concepts which are available on the `physionet-data.mimiciii_derived` dataset on BigQuery, and tutorials.
* [mimic-iv](https://github.com/MIT-LCP/mimic-code/blob/main/mimic-iv) - build scripts for MIMIC-IV, derived concepts which are available on the `physionet-data.mimic_derived` dataset on BigQuery, and tutorials.
* [mimic-iv-cxr](https://github.com/MIT-LCP/mimic-code/blob/main/mimic-iv-cxr) - code for loading and analyzing both dicom (mimic-iv-cxr/dcm) and text (mimic-iv-cxr/txt) data. In order to clearly indicate that MIMIC-CXR can be linked with MIMIC-IV, we have named this folder mimic-iv-cxr, and any references to MIMIC-CXR / MIMIC-IV-CXR are interchangeable.
* [mimic-iv-ed](https://github.com/MIT-LCP/mimic-code/blob/main/mimic-iv-ed) - build scripts for MIMIC-IV-ED.
* mimic-iv-waveforms - TBD

\






\
