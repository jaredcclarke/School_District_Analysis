# School_District_Analysis
## Overview
### Purpose
The purpose of this analysis was to replace the math and reading scores of ninth graders of Thomas High School (THS) with "NaNs," due to dishonesty with the school's grade reporting. After the scores were replaced, the next goal of the analysis was to perform another analysis of the data to show how the changes affedcted the overall analysis. 

## Resources
  ### Data Tools and Resources
  * Python 3.9 
  * `"Resources/schools_complete.csv"` 
  * `"Resources/students_complete.csv"` 
  * Anaconda 4.8.5 
  * Pandas
  * Jupyter Notebook 6.1.4

## Results
### District Summary
* First changed THS 9th grade reading and math scores scores to "NaN".
* A comparison operator was used to retrieve all the rows with Thomas High School in the "school_name" column of the `student_data_df`.

``` # Use the loc method on the student_data_df to select all the reading scores from the 9th grade at Thomas High School and replace them with NaN.
student_data_df.loc[(student_data_df["school_name"] == "Thomas High School") & (student_data_df["grade"] == "9th"), "reading_score"] = np.nan

# Refactor the code to replace the math scores with NaN.
student_data_df.loc[(student_data_df["school_name"] == "Thomas High School") & (student_data_df["grade"] == "9th"), "math_score"] = np.nan
```
* Used the `loc` method with logical and comparison operators, retrieve the student count for Thomas High School ninth graders in the school_data_complete_df DataFrame.

``` ths_9th_graders_count = student_data_df.loc[(student_data_df["school_name"] == "Thomas High School") & (student_data_df["grade"] == "9th"), ["Student ID"]].count()
```
* Subtracted the number of 9th grade students from the total student count to get the new total student count.

```# Get the total student count 
student_count = school_data_complete_df["Student ID"].count()

# Subtract the number of students that are in ninth grade at 
# Thomas High School from the total student count to get the new total student count.
total_student_count = student_count - ths_9th_graders_count
```
* Calculated the math and reading passing percentages based on the new total student count.

``` passing_math_count = school_data_complete_df[(school_data_complete_df["math_score"] >= 70)].count()["student_name"]
passing_reading_count = school_data_complete_df[(school_data_complete_df["reading_score"] >= 70)].count()["student_name"]
passing_math_percentage = passing_math_count / float(total_student_count) * 100
passing_reading_percentage = passing_reading_count / float(total_student_count) * 100
```
 * Calculated the overall passing percentage with the new total student count.
 ``` passing_math_reading = school_data_complete_df[(school_data_complete_df["math_score"] >= 70)
                    & (school_data_complete_df["reading_score"] >= 70)]
# Calculate the number of students that passed both reading and math.
overall_passing_math_reading_count = passing_math_reading["student_name"].count()

overall_passing_percentage = overall_passing_math_reading_count / float(total_student_count) * 100
```
<img width="1114" alt="new_district_summary" src="https://user-images.githubusercontent.com/70492224/96577401-93954500-12a1-11eb-9e1c-7645ca54113d.png">

### School Summary
* Counted the number of the passing students from the 10th-12th grade from THS.
``` 
ths_10th_to_12th = school_data_complete_df.loc[((school_data_complete_df["school_name"] == "Thomas High School") & (school_data_complete_df["grade"] != "9th"), "Student ID")].count()
```
* Used the `loc` method to create a new DataFrame that has all the students passing math from THS.
``` 
ths_math_passing = school_data_complete_df.loc[(school_data_complete_df["school_name"] == "Thomas High School") & (school_data_complete_df["math_score"] >= 70), "Student ID"].count()
```
* Used the `loc` method to create a new DataFrame that has all the students passing reading from THS.
```
ths_reading_passing = school_data_complete_df.loc[(school_data_complete_df["school_name"] == "Thomas High School") & (school_data_complete_df["reading_score"] >= 70), "Student ID"].count()
```
* Used the `loc` method to create a new DataFrame that has all the students passing math and reading from THS.
```
ths_overall_passing = school_data_complete_df.loc[(school_data_complete_df["school_name"] == "Thomas High School") & (school_data_complete_df["math_score"] >= 70) & (school_data_complete_df["reading_score"] >= 70), "Student ID"].count()
```
* Calculated the percentage of 10th-12th grade students passing math from THS.
```
percent_ths_pass_math = ths_math_passing / float(ths_10th_to_12th) * 100
```
* Calculated the percentage of 10th-12th grade students passing reading from Thomas High School.
```
percent_ths_pass_reading = ths_reading_passing / float(ths_10th_to_12th) * 100
```
* Calculated the overall passing percentage of 10th-12th grade students from Thomas High School.
```
percent_ths_overall_passing = ths_overall_passing / float(ths_10th_to_12th) * 100
```
* Used the `loc` method to replace the `% Passing Math` score for THS with the new math passing percentage.
```
per_school_summary_df.loc[["Thomas High School"], ["% Passing Math"]] = percent_ths_pass_math
```
* Used the `loc` method to replace the `% Passing Reading` score for THS with the new reading passing percentage.
```
per_school_summary_df.loc[["Thomas High School"], ["% Passing Reading"]] = percent_ths_pass_reading
```
* Used the `loc` method to replace the `% Overall Passing` score for THS with the new overall passing percentage.
```
per_school_summary_df.loc[["Thomas High School"], ["% Overall Passing"]] = percent_ths_overall_passing
```
<img width="1108" alt="new_school_summary" src="https://user-images.githubusercontent.com/70492224/96577673-f5ee4580-12a1-11eb-9505-75733cceeb5b.png">

### Top 5 and Bottom 5 Performing Schools
* Sorted the top 5 schools based on `% Overall Passing`.
```
top_schools = per_school_summary_df.sort_values(["% Overall Passing"], ascending=False)
```
<img width="1005" alt="top_5_schools" src="https://user-images.githubusercontent.com/70492224/96577806-351c9680-12a2-11eb-895b-af8697f96541.png">

* Sorted the bottom 5 schools based on `% Overall Passing`.
```
bottom_schools = per_school_summary_df.sort_values(["% Overall Passing"], ascending=True)
```
<img width="1006" alt="bottom_5_schools" src="https://user-images.githubusercontent.com/70492224/96577892-58474600-12a2-11eb-9dfb-a30b73c072d7.png">

### The Average Math and Reading Score for Each Grade Level By School
* The Average Math Score for each grade by school.
```
# Create a Series of scores by grade levels using conditionals.
ninth_graders = school_data_complete_df[(school_data_complete_df["grade"] == "9th")]

tenth_graders = school_data_complete_df[(school_data_complete_df["grade"] == "10th")]

eleventh_graders = school_data_complete_df[(school_data_complete_df["grade"] == "11th")]

twelfth_graders = school_data_complete_df[(school_data_complete_df["grade"] == "12th")]

ninth_grade_math_scores = ninth_graders.groupby(["school_name"]).mean()["math_score"]

tenth_grade_math_scores = tenth_graders.groupby(["school_name"]).mean()["math_score"]

eleventh_grade_math_scores = eleventh_graders.groupby(["school_name"]).mean()["math_score"]

twelfth_grade_math_scores = twelfth_graders.groupby(["school_name"]).mean()["math_score"]
```
<img width="322" alt="math_scores_by_grade" src="https://user-images.githubusercontent.com/70492224/96577983-790f9b80-12a2-11eb-8da6-a563441a29ea.png">

* The Average Reading Score for each grade by school.
```
ninth_grade_reading_scores = ninth_graders.groupby(["school_name"]).mean()["reading_score"]

tenth_grade_reading_scores = tenth_graders.groupby(["school_name"]).mean()["reading_score"]

eleventh_grade_reading_scores = eleventh_graders.groupby(["school_name"]).mean()["reading_score"]

twelfth_grade_reading_scores = twelfth_graders.groupby(["school_name"]).mean()["reading_score"]
```
<img width="309" alt="reading_score_by_grade" src="https://user-images.githubusercontent.com/70492224/96578044-92b0e300-12a2-11eb-88ad-51f49d4dddc6.png">

### Scores by School Spending Per Student, School Size and School Type
* Scores by school spending.
```
# Establish the spending group names.
spending_bins = [0, 585, 630, 645, 675]
group_names = ["<$584", "$585-629", "$630-644", "$645-675"]
group_names
# Categorize spending based on the bins.
per_school_summary_df["Spending Ranges (Per Student)"] = pd.cut(per_school_capita, spending_bins, labels=group_names)
# Calculate averages for the desired columns.
spending_math_scores = per_school_summary_df.groupby(["Spending Ranges (Per Student)"]).mean()["Average Math Score"]

spending_reading_scores = per_school_summary_df.groupby(["Spending Ranges (Per Student)"]).mean()["Average Reading Score"]

spending_passing_math = per_school_summary_df.groupby(["Spending Ranges (Per Student)"]).mean()["% Passing Math"]

spending_passing_reading = per_school_summary_df.groupby(["Spending Ranges (Per Student)"]).mean()["% Passing Reading"]

overall_passing_spending = per_school_summary_df.groupby(["Spending Ranges (Per Student)"]).mean()["% Overall Passing"]

# Establish the spending bins and group names.
spending_bins = [0, 585, 630, 645, 675]
group_names = ["<$584", "$585-629", "$630-644", "$645-675"]
```
<img width="1045" alt="scores_by_school_spending" src="https://user-images.githubusercontent.com/70492224/96578127-b116de80-12a2-11eb-8745-eef5248615d1.png">

* Scores by school size.
```
# Establish the bins.
size_bins = [0, 1000, 2000, 5000]
group_names = ["Small (<1000)", "Medium (1000-2000)", "Large (2000-5000)"]

per_school_summary_df["School Size"] = pd.cut(per_school_summary_df["Total Students"], size_bins, labels=group_names)
```
<img width="1019" alt="scores_by_school_size" src="https://user-images.githubusercontent.com/70492224/96578186-c5f37200-12a2-11eb-9bb3-57fe46525a8e.png">

* Scores by school type
```
# Calculate averages for the desired columns.
type_math_scores = per_school_summary_df.groupby(["School Type"]).mean()["Average Math Score"]

type_reading_scores = per_school_summary_df.groupby(["School Type"]).mean()["Average Reading Score"]

type_passing_math = per_school_summary_df.groupby(["School Type"]).mean()["% Passing Math"]

type_passing_reading = per_school_summary_df.groupby(["School Type"]).mean()["% Passing Reading"]

type_overall_passing = per_school_summary_df.groupby(["School Type"]).mean()["% Overall Passing"]
```
<img width="844" alt="Scores_by_school_type" src="https://user-images.githubusercontent.com/70492224/96578219-d4418e00-12a2-11eb-9e2b-d7d1bf104f35.png">

## Summary
After the math and reading scores for the ninth graders at Thomas High School were changed to "NaN," the folowing changes occured:
* The overall passing percentage of the the schools change minimally -- from 65.2% to 64.9% 
* Thomas High School's % passing scores were: 66.9% passing math score, 69.7% passing reading score and 65.1% overall passing score
* After removing the ninth graders from Thomas HIgh School's total students there are now 1174 students test results that are being analyzed in this data, instead of the original number of 1635
* After the removal of the ninth graders, the passing scores were: 93.2% passing math, 97% passing reading and 90.6% passing overall
* The changes to Thomas High SChool did not significantly affect the scores based on spending ranges per students
* It also did not affect the scores by school size or school type significantly.
