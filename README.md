Dataset Description  :


Case identifiers : case_id, patient_id, surgery_date, surgery_year, surgery_month, surgery_quarter, surgery_day_of_week

Hospital & staff : hospital_name, hospital_tier, surgical_specialty, surgeon_id, surgeon_experience

Patient profile : patient_age, patient_gender, patient_state, blood_group, bmi, smoker, diabetic, hypertensive, cardiac_history

Procedure : procedure_type, procedure_complexity, procedure_duration_min, anaesthesia_type, emergency_case, asa_score, procedure_weight, case_complexity_score

Outcomes : mortality_flag, complication_flag, complication_type, ssi_flag, return_to_or_flag, length_of_stay_days, icu_admission, icu_days, readmission_30d, discharge_status, follow_up_scheduled, follow_up_attended, patient_satisfaction_score

Financials : actual_cost_inr, insurance_type, insurance_covered_inr, out_of_pocket_inr

Quality metrics : expected_mortality, risk_adj_mortality_index, benchmark_los, los_vs_benchmark, benchmark_complication_rate, benchmark_mortality_index, benchmark_ssi_rate, quality_score


 

Dashboard  — Executive Clinical Overview 

          Calculated Fields :


[Mortality Rate %]
SUM([mortality_flag]) / COUNT([case_id]) * 100

[Complication Rate %]
SUM([complication_flag]) / COUNT([case_id]) * 100

[Risk-Adjusted Mortality Index]
SUM([mortality_flag]) / NULLIF(SUM([expected_mortality]), 0)

[SSI Rate per 1000]
SUM([ssi_flag]) / COUNT([case_id]) * 1000

[30-Day Readmission Rate %]
SUM([readmission_30d]) / COUNT([case_id]) * 100

[ICU Admission Rate %]
SUM([icu_admission]) / COUNT([case_id]) * 100

[Avg Length of Stay]
AVG([length_of_stay_days])

[LOS vs Benchmark]
AVG([length_of_stay_days]) - AVG([benchmark_los])

 [Mortality Index Status]
IF [Risk-Adjusted Mortality Index] > 1.20 THEN "High Risk"
ELSEIF [Risk-Adjusted Mortality Index] > 1.00 THEN "Above Expected"
ELSEIF [Risk-Adjusted Mortality Index] < 0.80 THEN "Excellent"
ELSE "As Expected"
END

[Quality Score Tier]
IF [quality_score] >= 90 THEN " Excellent"
ELSEIF [quality_score] >= 75 THEN " Good"
ELSEIF [quality_score] >= 60 THEN " Needs Improvement"
ELSE " Critical"
END

[YoY Mortality Change]
(SUM([mortality_flag]) / COUNT([case_id]))
LOOKUP(SUM([mortality_flag]) / COUNT([case_id]), -12)

[Date Range Filter]
[surgery_date] >= [Start Date]
AND [surgery_date] <= [End Date]



Dashboard  — Operations & Financial 


[Avg Case Cost INR]

AVG([actual_cost_inr])

[Insurance Coverage Rate %]
SUM([insurance_covered_inr]) / NULLIF(SUM([actual_cost_inr]), 0) * 100

[Avg Out-of-Pocket INR]

AVG([out_of_pocket_inr])

[Cost vs Complexity Index]

AVG([actual_cost_inr]) / NULLIF(AVG([case_complexity_score]), 0)

 [Emergency vs Elective Label]
IF [emergency_case] = 1
THEN "Emergency"
ELSE "Elective"
END

[ICU Utilisation Rate %]
SUM([icu_admission]) / COUNT([case_id]) * 100

[Avg ICU Days]
AVG([icu_days])


