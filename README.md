---
description: Halaman ini akan menjelaskan pendahuluan
---

# MIMIC-IV

## Abstract

Retrospectively collected medical data has the opportunity to improve patient care through knowledge discovery and algorithm development. Broad reuse of medical data is desirable for the greatest public good, but data sharing must be done in a manner which protects patient privacy. The Medical Information Mart for Intensive Care (MIMIC)-III database provided critical care data for over 40,000 patients admitted to intensive care units at the Beth Israel Deaconess Medical Center (BIDMC). Importantly, MIMIC-III was deidentified, and patient identifiers were removed according to the Health Insurance Portability and Accountability Act (HIPAA) Safe Harbor provision. MIMIC-III has been integral in driving large amounts of research in clinical informatics, epidemiology, and machine learning. Here we present MIMIC-IV, an update to MIMIC-III, which incorporates contemporary data and improves on numerous aspects of MIMIC-III. MIMIC-IV adopts a modular approach to data organization, highlighting data provenance and facilitating both individual and combined use of disparate data sources. MIMIC-IV is intended to carry on the success of MIMIC-III and support a broad set of applications within healthcare.

## Background

In recent years there has been a concerted move towards the adoption of digital health record systems in hospitals. In the US, nearly 96% of hospitals had a digital electronic health record system (EHR) in 2015 \[1]. Retrospectively collected medical data has increasingly been used for epidemiology and predictive modeling. The latter is in part due to the effectiveness of modeling approaches on large datasets \[2]. Despite these advances, access to medical data to improve patient care remains a significant challenge. While the reasons for limited sharing of medical data are multifaceted, concerns around patient privacy are highlighted as one of the most significant issues. Although patient studies have shown almost uniform agreement that deidentified medical data should be used to improve medical practice, domain experts continue to debate the optimal mechanisms of doing so. Uniquely, the MIMIC-III database adopted a permissive access scheme which allowed for broad reuse of the data \[3]. This mechanism has been successful in the wide use of MIMIC-III in a variety of studies ranging from assessment of treatment efficacy in well defined cohorts to prediction of key patient outcomes such as mortality. MIMIC-IV aims to carry on the success of MIMIC-III, with a number of changes to improve usability of the data and enable more research applications.

## Data Description

MIMIC-IV is grouped into two modules: _hosp_, and _icu_. The aim of these modules is to highlight their provenance.

### hosp

The _hosp_ module contains data derived from the hospital wide EHR. These measurements are predominantly recorded during the hospital stay, though some tables include data from outside the hospital as well (e.g. outpatient laboratory tests in _labevents_). Patient demographics (_patients_), hospitalizations (_admissions_), and intra-hospital transfers (_transfers_) are recorded in the _hosp_ module.

Notably, the _patients_ table provides timing information for each patient through the _anchor\_year_ and _anchor\_year\_group_ columns. The _anchor\_year_ is a deidentified year occurring sometime between 2100 - 2200, and the _anchor\_year\_group_ is a three year long date ranges between 2008 - 2019. These pieces of information allow researchers to infer the approximate year a patient received care. For example, if a patient's _anchor\_year_ is 2158, and their _anchor\_year\_group_ is 2011 - 2013, then any hospitalizations for the patient occurring in the year 2158 actually occurred sometime between 2011 - 2013. Finally, the _anchor\_age_ provides the patient age in the given _anchor\_year_. If the patient was over 89 in the _anchor\_year_, this _anchor\_age_ has been set to 91 (i.e. all patients over 89 have been grouped together into a single group with value 91, regardless of what their real age was).

Date of death is available within the _dod_ column of the _patients_ table. Date of death is derived from hospital records and state records. If both exist, hospital records take precedence. State records were matched using a custom rule based linkage algorithm based on name, date of birth, and social security number. State and hospital records for date of death were collected two years after the last patient discharge in MIMIC-IV, which should limit the impact of reporting delays in date of death.

Dates of death occurring more than one year after hospital discharge are censored as a part of the deidentification process. As a result, the maximum time of follow up for each patient is exactly one year after their last hospital discharge. For example, if a patient's last hospital discharge occurs on 2150-01-01, then the last possible date of death for the patient is 2151-01-01. If the individual died on or before 2151-01-01, and it was captured in either state or hospital death records, then the _dod_ column will contain the deidentified date of death. If the individual survived for at least one year after their last hospital discharge, then the _dod_ column will have a NULL value.

Other information in the _hosp_ module includes laboratory measurements (_labevents_, _d\_labitems_), microbiology cultures (_microbiologyevents_, _d\_micro_), provider orders (_poe_, _poe\_detail_), medication administration (_emar_, _emar\_detail_), medication prescription (_prescriptions_, _pharmacy_), hospital billing information (_diagnoses\_icd_, _d\_icd\_diagnoses_, _procedures\_icd_, _d\_icd\_procedures_, _hcpcsevents_, _d\_hcpcs_, _drgcodes_), online medical record data (_omr_), and service related information (_services_).

Provider information is available in the _provider_ table. The provider\_id column is a deidentified character string which uniquely represents a single care provider. As `provider_id` is used in different contexts across the module, a prefix is usually present in data tables to contextualize how the provider relates to the event. For example, the provider who admits the patient to the hospital is documented in the _admissions_ table as `admit_provider_id`. All columns which have a suffix of `provider_id` may be linked to the _provider_ table.

### icu

The _icu_ module contains data sourced from the clinical information system at the BIDMC: MetaVision (iMDSoft). MetaVision tables were denormalized to create a star schema where the _icustays_ and _d\_items_ tables link to a set of data tables all suffixed with "events". Data documented in the _icu_ module includes intravenous and fluid inputs (_inputevents_), ingredients for the aforementioned inputs (_ingredientevents),_ patient outputs (_outputevents_), procedures (_procedureevents_), information documented as a date or time (_datetimeevents_), and other charted information (_chartevents_). All events tables contain a _stay\_id_ column allowing identification of the associated ICU patient in _icustays_, and an _itemid_ column allowing identification of the concept documented in _d\_items_. Additionally, the _caregiver_ table contains `caregiver_id`, a deidentified integer representing the care provider who documented data into the system. All events tables (_chartevents_, _datetimeevents_, _ingredientevents_, _inputevents_, _outputevents_, _procedureevents_) have a `caregiver_id` column which links to the _caregiver_ table.

## Release Notes

### MIMIC-IV v2.2

MIMIC-IV v2.2 was released in January 2023. It added provider identifiers, imputed `hadm_id` for a number of rows in _emar_, and changed the subset of `subject_id` which are held out. Final row counts are available in the validation scripts published with the MIMIC Code Repository \[6]. For clarity, after removal of the test set, the row counts are as follows:

* _patients_: 299,712 (was 315,460 in v2.0)
* _admissions_: 431,231 (was 454,324 in v2.0)
* _icustays_: 73,181 (was 76,943 in v2.0)

#### **icu module**

* _caregiver_
  * New table in v2.2. Contains one column: `caregiver_id`, a deidentified integer which uniquely represents a single caregiver or provider. These identifiers are sourced from the MetaVision ICU system. When present in a table, it indicates the user who documented the data into MetaVision. For example, the `caregiver_id` associated with a row indicating mechanical ventilation in the procedureevents table represents the user who documented the event, and not the provider who performed the procedure.
* _chartevents_, _datetimeevents_, _ingredientevents_, _inputevents_, _outputevents_, _procedureevents_
  * Added the `caregiver_id` column. This column is a deidentified integer representing the care provider who documented the data for the given row.

#### **hosp module**

* _provider_
  * New table in v2.2. Contains one column: `provider_id`, a deidentified string which uniquely represents a single caregiver or provider. These identifiers are sourced from the hospital wide EHR system, and used in a variety of contexts across tables in the module.
* _admissions_
  * New column: `admit_provider_id`, a deidentified string representing the provider who admitted the patient.
* _emar_
  * New column: `enter_provider_id`, a deidentified string representing the provider who entered the medication administration information into the database.
  * Fixed a bug where a subset of _emar_ rows (713,117, \~2.5%) did not have an `hadm_id` even though they were associated with a given hospitalization. These rows occur outside of the administratively documented admission and discharge times for a hospitalization, but are still considered as administered during that hospitalization in the raw data.
* _labevents_, _microbiologyevents_, _poe_, _prescriptions_
  * New column: `order_provider_id`, a deidentified string representing the provider who ordered the corresponding event (e.g. the lab test in the case of _labevents_, or the medication in the case of _prescriptions_).

## Reference

| Johnson, A., Bulgarelli, L., Pollard, T., Horng, S., Celi, L. A., and Mark, R. (2023) 'MIMIC-IV' (version 2.2), _PhysioNet_. Available at: [https://doi.org/10.13026/6mm1-ek67](https://doi.org/10.13026/6mm1-ek67).                                                         |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Goldberger, A., Amaral, L., Glass, L., Hausdorff, J., Ivanov, P. C., Mark, R., ... & Stanley, H. E. (2000). PhysioBank, PhysioToolkit, and PhysioNet: Components of a new research resource for complex physiologic signals. Circulation \[Online]. 101 (23), pp. e215–e220. |

\
