# Healthcare-Analytics

In this project, I play the role of a healthcare data analyst that guides strategic decisions at a local hospital. The hospital's management team expressed concerns about resource allocation and patient care efficiency across different departments. 
My goal was to provide actionable insights into patients' demographics, diagnosis, and visit patterns to optimize service delivery and enhance patient outcomes.

Insights and Recommendations are provided on the following key areas:
1. What is the demographic profile of the patient population?
2. Which diagnoses are most prevalent among patients?
3. What are the most common appointment times throughout the day?
4. What are the most commonly ordered lab tests?
5. Typically, fasting blood sugar levels fall between 70-100 mg/dL. The goal is to identify patients 
whose lab results are outside this normal range to implement early intervention.
6. Assess how many patients are considered High, Medium, and Low Risk.
7. Patients who had multiple visits within 30 days of their previous medical visit.

The sql queries the whole process from data cleaning to analysis is attached to the files.

## Data Structure
The data used for this project consists of 5 tables: Hospital Records, Patients table, Outpatient Visits, Lab Results and Appointment Analysis tables. I created a new database named Healthcare in the SQL Server Management Studio and imported the 5 tables.

## Data Cleaning
1. Removed null rows from the Patients and Appointments tables.
2. Checked all tables for duplicates
3. The diagnosis and medication fields in the outpatient table had some null values. As I explored the dataset further, I noticed that patients with missing diagnosis and medication prescribed had their reason for hospital visit as checkups, annual physical or counselling. This could imply that no illness was identified and hence the absence of a diagnosis and or medication prescription. I replaced the missing values with 'N/A'.
4. I adopted an iterative process for changing data types due to the number of tables. As and when I had to convert, I did during my analysis.

 ## Data Analysis
 
### Approach
- The first requirement was to retrieve information on the demographic profile of the patient population, including age and gender distribution. Patients less than 17 years old were to be considered paediatric, patients between 18 and 64, adults, and patients more than 65, seniors. 

Process: I wrote a CASE statement to create a new column, categorizing patients into their respective categories based on their age groupings.
There was no age included in the patients table. To obtain their ages, I used the DATEDIFF function to find the difference between their date of birth and the current date.

- The next requirement was to examine which diagnoses were most prevalent among patients and how they varied across the different demographic groups, including gender and age.

Process: The diagnoses were located in the Outpatients visits table. To be able to return the diagnoses alongside the demographic profile, I created an inner join between the patients and outpatients visits tables to merge both tables using the patient_id as the field to match on. I then aliased both tables for easy readability. 

- Thirdly, the hospital management team sought to know the most common appointment times throughout the day and how the distribution of appointment times varied across different hours.

Process: To obtain the specific appointment hour, I extracted the hour from the appointment time using the DATEPART function, extracted the day of week from the date using the DATENAME function and sorted by the number of appointments in descending order to identify the most common appointment times.

- To identify patients whose fasting blood sugar levels were not within the normal range:

Process: I first performed an inner join of the lab results table with the outpatients visit table and another inner join of the outpatients table with the patients table. This is because, patient info like the patient id and name were located in the patients table, but there was no matching column directly between the patients table and lab results. 
To be able to retrieve patient info alongside the lab results, the double inner joins had to be performed. I then selected the appropriate columns using their respective aliases and filtered the selection using the WHERE clause to return only patients who took the fasting blood sugar test and had a result value < 70 or > 100.

- Cardiovascular disease is linked to smoking, among other conditions. The hospital management wants to prevent cardiovascular disease, and they need to access how many patients are considered high, medium, and low risk. 

High risk: patients who are smokers and have been diagnosed with either hypertension or diabetes. Medium risk: patients who are non-smokers and have been diagnosed with either hypertension or diabetes. 

Low risk, patients who do not fall into the high or medium risk categories.

Process: Diagnoses and smoker status info were located in the Outpatient visits table. From the outpatients visits table, I wrote a CASE statement to create a new column, categorizing patients into their respective risk categories based on their smoker status and respective diagnoses.

- Lastly, the hospital administration was interested in finding out information about the patients who were readmitted within 30 days of their previous medical visit. They wanted to know the reason for the initial visit and the readmission visit. 

Process: To be able to return the admission and readmission dates for a particular patient alongside each other, I performed a self join using the Outpatients Visits table. I then selected the appropriate columns using their respective table aliases.

## Dashboard Design
The stakeholders involved are the hospital management. This informed the simplicity of the dashboard with a focus on summary and high level metrics with minimal interactivity. The sql queries were used to generate base reports for each visual. This ensured that, each visual addressed the specific problem required by the hospital management.
The tool tip gives information on the key insights and recommendations for problem solving.
