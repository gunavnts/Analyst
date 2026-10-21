# Clinic Patient Visits Dashboard

A one-page dashboard that summarizes the clinic's patient population, diagnoses, visit utilization and procedures, built from `patient_visits.csv`.

## Business questions

1. What does the patient population look like by age?
2. What are the top diagnoses overall, and how do they differ by age band and sex?
3. How many visits does the average patient have, and who are the high utilizers?
4. Which CPT (procedure) codes are performed most often?

## Files

| File | Purpose |
|---|---|
| `patient_visits.csv` | Source data (200 rows, one row per diagnosis event) |
| `clinic_dashboard.html` | One-page dashboard (also published as a Claude artifact) |
| `tableau_ready_data.csv` | Cleaned data for Tableau, with calculated fields added |
| `README.md` | This file |

## Data dictionary

| Column | Description |
|---|---|
| `visit_id` | Visit identifier |
| `patient_id` | Patient identifier |
| `visit_date` | Date of visit (Jan 2022 – Dec 2024) |
| `date_of_birth` | Patient date of birth |
| `patient_age` | Age as supplied in the source (not used, see Data quality) |
| `patient_sex` | Female / Male |
| `icd_code` | Diagnosis code (ICD-10) |
| `cpt_code` | Procedure code (CPT) |

`tableau_ready_data.csv` adds two columns: `visits_for_patient` (total events for that patient ID) and `high_utilizer` (True if 4 or more).

## Method

- **Age:** calculated from `date_of_birth` as of Oct 21, 2026 (the dashboard build date), not taken from `patient_age`.
- **Age bands:** 0–17, 18–39, 40–64, 65+.
- **Patient counts:** one row per unique `patient_id`. Where an ID has more than one date of birth or sex, the most common value is used.
- **Visits:** each row counts as one visit or diagnosis event. Average visits per patient = total rows ÷ unique patients.
- **High utilizers:** patients with 4 or more visits.
- **Top diagnoses and procedures:** frequency counts of `icd_code` and `cpt_code` across all rows, top 10 shown. The by-age-band breakdown uses each row's own date of birth; the by-sex breakdown uses each row's `patient_sex`.

## Key findings

- 42 patients and 200 diagnosis events.
- Age bands: 0–17: 0, 18–39: 8, 40–64: 16, 65+: 18.
- Top diagnoses: I10 (19), J45.909 (11), N39.0 (11), E11.9 (10), M54.5 (10), E78.5 (10).
- Average of 4.76 visits per patient; 20 patients (48%) are high utilizers.
- Most frequent CPT codes: 99213 (7), 99214 (6), 99204 (6); most others appear 4 times.

## Data quality notes

These affect how far the numbers can be trusted:

- **Reused patient IDs:** 20 patient IDs (PT00001 to about PT00022) each appear 8–9 times with different dates of birth, sex and ages, and visit IDs repeat across them. They likely represent several different people sharing an ID, which would overstate the high-utilizer count and the average visits per patient. The IDs should be verified with the hospital.
- **`patient_age` does not match `date_of_birth`:** the supplied age is off by about 2–5 years for most rows, so it was ignored.
- **CPT codes look like placeholders:** many are sequential values (12345, 23456, 34567 and similar), so the ranking beyond the top three has little meaning.
- **No minors:** no patients fall in the 0–17 band, so it shows as zero.

## Using the data in Tableau

1. Connect to `tableau_ready_data.csv`.
2. Create a calculated field for age band, for example `IF [Age] < 18 THEN "0–17" ELSEIF [Age] < 40 THEN "18–39" ELSEIF [Age] < 65 THEN "40–64" ELSE "65+" END`, using `DATEDIFF('year', [date_of_birth], TODAY())` for age.
3. Build the views:
   - Bar chart of count distinct `patient_id` by age band.
   - Bar chart of count of `icd_code`, limited to the top 10, with age band and sex as filters or a highlight table.
   - Table of `patient_id` filtered on `high_utilizer = True`.
   - Bar chart of count of `cpt_code`, limited to the top 10.
4. Combine the views on one dashboard.

## Dashboard layout

Top row: title and four summary numbers (patients, events, average visits, high utilizers). Middle row: age bands, visits per patient, top 10 diagnoses, top 10 CPT codes. Bottom row: top diagnoses by age band, top diagnoses by sex, and the high-utilizer table.
