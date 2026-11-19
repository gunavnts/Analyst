Hospital Operations Analytics | Power BI
A Power BI portfolio project that uses synthetic 2025 hospital data to analyze patient activity, wait times, outpatient no-shows, and bed occupancy across six departments.
Data and approach
- Source data. Four CSVs contain 11,017 encounter records, 1,800 patient records, six departments, and 2,190 daily department-level bed snapshots.
- Power Query. Set data types, standardized payer categories, checked lookup keys and bed capacity, and derived inpatient stay days from visit and discharge dates. Kept missing wait times blank.
- Data model. Used shared Calendar and Departments tables to filter both Encounters and DailyBeds. Linked Patients to Encounters by patient ID.
- DAX. Built measures for completed visits, unique patients, average wait time, average inpatient stay, outpatient no-show rate, and bed occupancy. Occupancy is total occupied bed-days divided by total available bed-days.
Report pages
Page	What it shows
Operations Overview	Visit KPIs, monthly completed visits, and average wait by department
Appointments & Capacity	No-show rates, inpatient stay length, monthly occupancy, and a department-by-month occupancy heatmap
Emergency Analysis	Daily average wait versus bed occupancy, with February 2025 as the starting view


Key findings
- Emergency had the longest average wait at 49.85 minutes, based on 2,369 completed visits with recorded wait times.
- Emergency had the highest department-month occupancy at 77.90% in February, compared with 71.22% hospital-wide that month.
- General Surgery had the highest outpatient no-show rate at 10.10%, with 107 missed visits out of 1,059 outpatient appointments with a completed or no-show status.
The findings describe the simulated hospital. The data generator built in a relationship between high Emergency occupancy and longer waits.
