Business Intelligence Project
================
<Specify your name here>
<Specify the date when you submitted the lab>

- [Student Details](#student-details)
- [Setup Chunk](#setup-chunk)
- [Understanding the Dataset (Exploratory Data Analysis
  (EDA))](#understanding-the-dataset-exploratory-data-analysis-eda)
  - [Loading the Dataset](#loading-the-dataset)
    - [Source:](#source)
    - [Reference:](#reference)
- [BEFORE](#before)
- [Calculate the skewness before the Box-Cox
  transform](#calculate-the-skewness-before-the-box-cox-transform)
- [AFTER](#after)
- [Calculate the skewness after the Box-Cox
  transform](#calculate-the-skewness-after-the-box-cox-transform)

# Student Details

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 66%" />
</colgroup>
<tbody>
<tr class="odd">
<td><strong>Student ID Numbers and Names of Group Members</strong></td>
<td><p><em>&lt;list one student name, group, and ID per line; you should
be between 2 and 5 members per group&gt;</em></p>
<ol type="1">
<li><p>135232 - Starfield - Sadiki Hamisi</p></li>
<li><p>134782 - Starfield - Yasmin Choma</p></li>
<li><p>134783 - Starfield - Moses mbugua</p></li>
<li><p>122998 - Starfield - Glenn Oloo</p></li>
</ol></td>
</tr>
<tr class="even">
<td><strong>GitHub Classroom Group Name</strong></td>
<td>Starfield</td>
</tr>
<tr class="odd">
<td><strong>Course Code</strong></td>
<td>BBT4206</td>
</tr>
<tr class="even">
<td><strong>Course Name</strong></td>
<td>Business Intelligence II</td>
</tr>
<tr class="odd">
<td><strong>Program</strong></td>
<td>Bachelor of Business Information Technology</td>
</tr>
<tr class="even">
<td><strong>Semester Duration</strong></td>
<td>21<sup>st</sup> August 2023 to 28<sup>th</sup> November 2023</td>
</tr>
</tbody>
</table>

# Setup Chunk

**Note:** the following KnitR options have been set as the global
defaults: <BR>
`knitr::opts_chunk$set(echo = TRUE, warning = FALSE, eval = TRUE, collapse = FALSE, tidy = TRUE)`.

More KnitR options are documented here
<https://bookdown.org/yihui/rmarkdown-cookbook/chunk-options.html> and
here <https://yihui.org/knitr/options/>.

# Understanding the Dataset (Exploratory Data Analysis (EDA))

## Loading the Dataset

### Source:

The dataset that was used can be downloaded here: *\<provide a link\>*

### Reference:

*\<Cite the dataset here using APA\>  
Refer to the APA 7th edition manual for rules on how to cite datasets:
<https://apastyle.apa.org/style-grammar-guidelines/references/examples/data-set-references>*

``` r
library(readr)
library(dplyr)
```

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
student_performance_dataset <-
  read_csv("data/student_performance_dataset.csv",
           col_types =
             cols(
               class_group = col_factor(levels = c("A", "B", "C")),
               gender = col_factor(levels = c("1", "0")),
               YOB = col_date(format = "%Y"),
               regret_choosing_bi = col_factor(levels = c("1", "0")),
               drop_bi_now = col_factor(levels = c("1", "0")),
               motivator = col_factor(levels = c("1", "0")),
               read_content_before_lecture =
                 col_factor(levels = c("1", "2", "3", "4", "5")),
               anticipate_test_questions =
                 col_factor(levels = c("1", "2", "3", "4", "5")),
               answer_rhetorical_questions =
                 col_factor(levels = c("1", "2", "3", "4", "5")),
               find_terms_I_do_not_know =
                 col_factor(levels = c("1", "2", "3", "4", "5")),
               copy_new_terms_in_reading_notebook =
                 col_factor(levels = c("1", "2", "3", "4", "5")),
               take_quizzes_and_use_results =
                 col_factor(levels = c("1", "2", "3", "4", "5")),
               reorganise_course_outline =
                 col_factor(levels = c("1", "2", "3", "4", "5")),
               write_down_important_points =
                 col_factor(levels = c("1", "2", "3", "4", "5")),
               space_out_revision =
                 col_factor(levels = c("1", "2", "3", "4", "5")),
               studying_in_study_group =
                 col_factor(levels = c("1", "2", "3", "4", "5")),
               schedule_appointments =
                 col_factor(levels = c("1", "2", "3", "4", "5")),
               goal_oriented = col_factor(levels = c("1", "0")),
               spaced_repetition =
                 col_factor(levels = c("1", "2", "3", "4")),
               testing_and_active_recall =
                 col_factor(levels = c("1", "2", "3", "4")),
               interleaving = col_factor(levels = c("1", "2", "3", "4")),
               categorizing = col_factor(levels = c("1", "2", "3", "4")),
               retrospective_timetable =
                 col_factor(levels = c("1", "2", "3", "4")),
               cornell_notes = col_factor(levels = c("1", "2", "3", "4")),
               sq3r = col_factor(levels = c("1", "2", "3", "4")),
               commute = col_factor(levels = c("1", "2", "3", "4")),
               study_time = col_factor(levels = c("1", "2", "3", "4")),
               repeats_since_Y1 = col_integer(),
               paid_tuition = col_factor(levels = c("0", "1")),
               free_tuition = col_factor(levels = c("0", "1")),
               extra_curricular = col_factor(levels = c("0", "1")),
               sports_extra_curricular = col_factor(levels = c("0", "1")),
               exercise_per_week = col_factor(levels = c("0", "1", "2", "3")),
               meditate = col_factor(levels = c("0", "1", "2", "3")),
               pray = col_factor(levels = c("0", "1", "2", "3")),
               internet = col_factor(levels = c("0", "1")),
               laptop = col_factor(levels = c("0", "1")),
               family_relationships =
                 col_factor(levels = c("1", "2", "3", "4", "5")),
               friendships = col_factor(levels = c("1", "2", "3", "4", "5")),
               romantic_relationships =
                 col_factor(levels = c("0", "1", "2", "3", "4")),
               spiritual_wellnes =
                 col_factor(levels = c("1", "2", "3", "4", "5")),
               financial_wellness =
                 col_factor(levels = c("1", "2", "3", "4", "5")),
               health = col_factor(levels = c("1", "2", "3", "4", "5")),
               day_out = col_factor(levels = c("0", "1", "2", "3")),
               night_out = col_factor(levels = c("0", "1", "2", "3")),
               alcohol_or_narcotics =
                 col_factor(levels = c("0", "1", "2", "3")),
               mentor = col_factor(levels = c("0", "1")),
               mentor_meetings = col_factor(levels = c("0", "1", "2", "3")),
               `Attendance Waiver Granted: 1 = Yes, 0 = No` =
                 col_factor(levels = c("0", "1")),
               GRADE = col_factor(levels = c("A", "B", "C", "D", "E"))),
           locale = locale())

View(student_performance_dataset)

# Dimensions
dim(student_performance_dataset)
```

    ## [1] 201 100

``` r
# Data Types
sapply(student_performance_dataset, class)
```

    ##                                                                                                                                                                                                                                                                                   class_group 
    ##                                                                                                                                                                                                                                                                                      "factor" 
    ##                                                                                                                                                                                                                                                                                        gender 
    ##                                                                                                                                                                                                                                                                                      "factor" 
    ##                                                                                                                                                                                                                                                                                           YOB 
    ##                                                                                                                                                                                                                                                                                        "Date" 
    ##                                                                                                                                                                                                                                                                            regret_choosing_bi 
    ##                                                                                                                                                                                                                                                                                      "factor" 
    ##                                                                                                                                                                                                                                                                                   drop_bi_now 
    ##                                                                                                                                                                                                                                                                                      "factor" 
    ##                                                                                                                                                                                                                                                                                     motivator 
    ##                                                                                                                                                                                                                                                                                      "factor" 
    ##                                                                                                                                                                                                                                                                   read_content_before_lecture 
    ##                                                                                                                                                                                                                                                                                      "factor" 
    ##                                                                                                                                                                                                                                                                     anticipate_test_questions 
    ##                                                                                                                                                                                                                                                                                      "factor" 
    ##                                                                                                                                                                                                                                                                   answer_rhetorical_questions 
    ##                                                                                                                                                                                                                                                                                      "factor" 
    ##                                                                                                                                                                                                                                                                      find_terms_I_do_not_know 
    ##                                                                                                                                                                                                                                                                                      "factor" 
    ##                                                                                                                                                                                                                                                            copy_new_terms_in_reading_notebook 
    ##                                                                                                                                                                                                                                                                                      "factor" 
    ##                                                                                                                                                                                                                                                                  take_quizzes_and_use_results 
    ##                                                                                                                                                                                                                                                                                      "factor" 
    ##                                                                                                                                                                                                                                                                     reorganise_course_outline 
    ##                                                                                                                                                                                                                                                                                      "factor" 
    ##                                                                                                                                                                                                                                                                   write_down_important_points 
    ##                                                                                                                                                                                                                                                                                      "factor" 
    ##                                                                                                                                                                                                                                                                            space_out_revision 
    ##                                                                                                                                                                                                                                                                                      "factor" 
    ##                                                                                                                                                                                                                                                                       studying_in_study_group 
    ##                                                                                                                                                                                                                                                                                      "factor" 
    ##                                                                                                                                                                                                                                                                         schedule_appointments 
    ##                                                                                                                                                                                                                                                                                      "factor" 
    ##                                                                                                                                                                                                                                                                                 goal_oriented 
    ##                                                                                                                                                                                                                                                                                      "factor" 
    ##                                                                                                                                                                                                                                                                             spaced_repetition 
    ##                                                                                                                                                                                                                                                                                      "factor" 
    ##                                                                                                                                                                                                                                                                     testing_and_active_recall 
    ##                                                                                                                                                                                                                                                                                      "factor" 
    ##                                                                                                                                                                                                                                                                                  interleaving 
    ##                                                                                                                                                                                                                                                                                      "factor" 
    ##                                                                                                                                                                                                                                                                                  categorizing 
    ##                                                                                                                                                                                                                                                                                      "factor" 
    ##                                                                                                                                                                                                                                                                       retrospective_timetable 
    ##                                                                                                                                                                                                                                                                                      "factor" 
    ##                                                                                                                                                                                                                                                                                 cornell_notes 
    ##                                                                                                                                                                                                                                                                                      "factor" 
    ##                                                                                                                                                                                                                                                                                          sq3r 
    ##                                                                                                                                                                                                                                                                                      "factor" 
    ##                                                                                                                                                                                                                                                                                       commute 
    ##                                                                                                                                                                                                                                                                                      "factor" 
    ##                                                                                                                                                                                                                                                                                    study_time 
    ##                                                                                                                                                                                                                                                                                      "factor" 
    ##                                                                                                                                                                                                                                                                              repeats_since_Y1 
    ##                                                                                                                                                                                                                                                                                     "integer" 
    ##                                                                                                                                                                                                                                                                                  paid_tuition 
    ##                                                                                                                                                                                                                                                                                      "factor" 
    ##                                                                                                                                                                                                                                                                                  free_tuition 
    ##                                                                                                                                                                                                                                                                                      "factor" 
    ##                                                                                                                                                                                                                                                                              extra_curricular 
    ##                                                                                                                                                                                                                                                                                      "factor" 
    ##                                                                                                                                                                                                                                                                       sports_extra_curricular 
    ##                                                                                                                                                                                                                                                                                      "factor" 
    ##                                                                                                                                                                                                                                                                             exercise_per_week 
    ##                                                                                                                                                                                                                                                                                      "factor" 
    ##                                                                                                                                                                                                                                                                                      meditate 
    ##                                                                                                                                                                                                                                                                                      "factor" 
    ##                                                                                                                                                                                                                                                                                          pray 
    ##                                                                                                                                                                                                                                                                                      "factor" 
    ##                                                                                                                                                                                                                                                                                      internet 
    ##                                                                                                                                                                                                                                                                                      "factor" 
    ##                                                                                                                                                                                                                                                                                        laptop 
    ##                                                                                                                                                                                                                                                                                      "factor" 
    ##                                                                                                                                                                                                                                                                          family_relationships 
    ##                                                                                                                                                                                                                                                                                      "factor" 
    ##                                                                                                                                                                                                                                                                                   friendships 
    ##                                                                                                                                                                                                                                                                                      "factor" 
    ##                                                                                                                                                                                                                                                                        romantic_relationships 
    ##                                                                                                                                                                                                                                                                                      "factor" 
    ##                                                                                                                                                                                                                                                                             spiritual_wellnes 
    ##                                                                                                                                                                                                                                                                                      "factor" 
    ##                                                                                                                                                                                                                                                                            financial_wellness 
    ##                                                                                                                                                                                                                                                                                      "factor" 
    ##                                                                                                                                                                                                                                                                                        health 
    ##                                                                                                                                                                                                                                                                                      "factor" 
    ##                                                                                                                                                                                                                                                                                       day_out 
    ##                                                                                                                                                                                                                                                                                      "factor" 
    ##                                                                                                                                                                                                                                                                                     night_out 
    ##                                                                                                                                                                                                                                                                                      "factor" 
    ##                                                                                                                                                                                                                                                                          alcohol_or_narcotics 
    ##                                                                                                                                                                                                                                                                                      "factor" 
    ##                                                                                                                                                                                                                                                                                        mentor 
    ##                                                                                                                                                                                                                                                                                      "factor" 
    ##                                                                                                                                                                                                                                                                               mentor_meetings 
    ##                                                                                                                                                                                                                                                                                      "factor" 
    ##                                                                                                                                                                                                                                                              A - 1. I am enjoying the subject 
    ##                                                                                                                                                                                                                                                                                     "numeric" 
    ##                                                                                                                                                                                                                                                          A - 2. Classes start and end on time 
    ##                                                                                                                                                                                                                                                                                     "numeric" 
    ##                                                                                                                                                                                               A - 3. The learning environment is participative, involves learning by doing and is group-based 
    ##                                                                                                                                                                                                                                                                                     "numeric" 
    ##                                                                                                                                                                                             A - 4. The subject content is delivered according to the course outline and meets my expectations 
    ##                                                                                                                                                                                                                                                                                     "numeric" 
    ##                                                                                                                                                                                                                                           A - 5. The topics are clear and logically developed 
    ##                                                                                                                                                                                                                                                                                     "numeric" 
    ##                                                                                                                                                                                                                                             A - 6. I am developing my oral and writing skills 
    ##                                                                                                                                                                                                                                                                                     "numeric" 
    ##                                                                                                                                                                                                                            A - 7. I am developing my reflective and critical reasoning skills 
    ##                                                                                                                                                                                                                                                                                     "numeric" 
    ##                                                                                                                                                                                                                                       A - 8. The assessment methods are assisting me to learn 
    ##                                                                                                                                                                                                                                                                                     "numeric" 
    ##                                                                                                                                                                                                                                                            A - 9. I receive relevant feedback 
    ##                                                                                                                                                                                                                                                                                     "numeric" 
    ##                                                                                                                                                                                                                                             A - 10. I read the recommended readings and notes 
    ##                                                                                                                                                                                                                                                                                     "numeric" 
    ##                                                                                                                                                                                                                                                   A - 11. I use the eLearning material posted 
    ##                                                                                                                                                                                                                                                                                     "numeric" 
    ##                                                                                                                                                                                                         B - 1. Concept 1 of 6: Principles of Business Intelligence and the DataOps Philosophy 
    ##                                                                                                                                                                                                                                                                                     "numeric" 
    ##                                                                                                                                                                                                                             B - 2. Concept 3 of 6: Linear Algorithms for Predictive Analytics 
    ##                                                                                                                                                                                                                                                                                     "numeric" 
    ##                                                                                                                                                                                                                                                     C - 2. Quizzes at the end of each concept 
    ##                                                                                                                                                                                                                                                                                     "numeric" 
    ##                                                                                                                                                                                                                           C - 3. Lab manuals that outline the steps to follow during the labs 
    ##                                                                                                                                                                                                                                                                                     "numeric" 
    ##                                                                                                                                                                           C - 4. Required lab work submissions at the end of each lab manual that outline the activity to be done on your own 
    ##                                                                                                                                                                                                                                                                                     "numeric" 
    ##                                                                                                                                                                                                                                                          C - 5. Supplementary videos to watch 
    ##                                                                                                                                                                                                                                                                                     "numeric" 
    ##                                                                                                                                                                                                                                                    C - 6. Supplementary podcasts to listen to 
    ##                                                                                                                                                                                                                                                                                     "numeric" 
    ##                                                                                                                                                                                                                                                          C - 7. Supplementary content to read 
    ##                                                                                                                                                                                                                                                                                     "numeric" 
    ##                                                                                                                                                                                                                                                                        C - 8. Lectures slides 
    ##                                                                                                                                                                                                                                                                                     "numeric" 
    ##                                                                                                                                                                                                                                            C - 9. Lecture notes on some of the lecture slides 
    ##                                                                                                                                                                                                                                                                                     "numeric" 
    ## C - 10. The quality of the lectures given (quality measured by the breadth (the full span of knowledge of a subject) and depth (the extent to which specific topics are focused upon, amplified, and explored) of learning - NOT quality measured by how fun/comical/lively the lectures are) 
    ##                                                                                                                                                                                                                                                                                     "numeric" 
    ##                                                                                                              C - 11. The division of theory and practice such that most of the theory is done during the recorded online classes and most of the practice is done during the physical classes 
    ##                                                                                                                                                                                                                                                                                     "numeric" 
    ##                                                                                                                                                                                                                                                      C - 12. The recordings of online classes 
    ##                                                                                                                                                                                                                                                                                     "numeric" 
    ##                                                                                                                                                                                                     D - 1. \r\nWrite two things you like about the teaching and learning in this unit so far. 
    ##                                                                                                                                                                                                                                                                                   "character" 
    ##                                                                                                                                                          D - 2. Write at least one recommendation to improve the teaching and learning in this unit (for the remaining weeks in the semester) 
    ##                                                                                                                                                                                                                                                                                   "character" 
    ##                                                                                                                                                                                                                                                              Average Course Evaluation Rating 
    ##                                                                                                                                                                                                                                                                                     "numeric" 
    ##                                                                                                                                                                                                                                                     Average Level of Learning Attained Rating 
    ##                                                                                                                                                                                                                                                                                     "numeric" 
    ##                                                                                                                                                                                                                                             Average Pedagogical Strategy Effectiveness Rating 
    ##                                                                                                                                                                                                                                                                                     "numeric" 
    ##                                                                                                                                                                                                                                                              Project: Section 1-4: (20%) x/10 
    ##                                                                                                                                                                                                                                                                                     "numeric" 
    ##                                                                                                                                                                                                                                                             Project: Section 5-11: (50%) x/10 
    ##                                                                                                                                                                                                                                                                                     "numeric" 
    ##                                                                                                                                                                                                                                                                Project: Section 12: (30%) x/5 
    ##                                                                                                                                                                                                                                                                                     "numeric" 
    ##                                                                                                                                                                                                                                                              Project: (10%): x/30 x 100 TOTAL 
    ##                                                                                                                                                                                                                                                                                     "numeric" 
    ##                                                                                                                                                                                                                                                       Quiz 1 on Concept 1 (Introduction) x/32 
    ##                                                                                                                                                                                                                                                                                     "numeric" 
    ##                                                                                                                                                                                                                                                             Quiz 3 on Concept 3 (Linear) x/15 
    ##                                                                                                                                                                                                                                                                                     "numeric" 
    ##                                                                                                                                                                                                                                                         Quiz 4 on Concept 4 (Non-Linear) x/22 
    ##                                                                                                                                                                                                                                                                                     "numeric" 
    ##                                                                                                                                                                                                                                                       Quiz 5 on Concept 5 (Dashboarding) x/10 
    ##                                                                                                                                                                                                                                                                                     "numeric" 
    ##                                                                                                                                                                                                                                               Quizzes and  Bonus Marks (7%): x/79 x 100 TOTAL 
    ##                                                                                                                                                                                                                                                                                     "numeric" 
    ##                                                                                                                                                                                                                                                 Lab 1 - 2.c. - (Simple Linear Regression) x/5 
    ##                                                                                                                                                                                                                                                                                     "numeric" 
    ##                                                                                                                                                                                                                                Lab 2 - 2.e. -  (Linear Regression using Gradient Descent) x/5 
    ##                                                                                                                                                                                                                                                                                     "numeric" 
    ##                                                                                                                                                                                                                               Lab 3 - 2.g. - (Logistic Regression using Gradient Descent) x/5 
    ##                                                                                                                                                                                                                                                                                     "numeric" 
    ##                                                                                                                                                                                                                                             Lab 4 - 2.h. - (Linear Discriminant Analysis) x/5 
    ##                                                                                                                                                                                                                                                                                     "numeric" 
    ##                                                                                                                                                                                                                                                          Lab 5 - Chart JS Dashboard Setup x/5 
    ##                                                                                                                                                                                                                                                                                     "numeric" 
    ##                                                                                                                                                                                                                                                                      Lab Work (7%) x/25 x 100 
    ##                                                                                                                                                                                                                                                                                     "numeric" 
    ##                                                                                                                                                                                                                                                                        CAT 1 (8%): x/38 x 100 
    ##                                                                                                                                                                                                                                                                                     "numeric" 
    ##                                                                                                                                                                                                                                                                       CAT 2 (8%): x/100 x 100 
    ##                                                                                                                                                                                                                                                                                     "numeric" 
    ##                                                                                                                                                                                                                                                    Attendance Waiver Granted: 1 = Yes, 0 = No 
    ##                                                                                                                                                                                                                                                                                      "factor" 
    ##                                                                                                                                                                                                                                                                        Absenteeism Percentage 
    ##                                                                                                                                                                                                                                                                                     "numeric" 
    ##                                                                                                                                                                                                                                                                  Coursework TOTAL: x/40 (40%) 
    ##                                                                                                                                                                                                                                                                                     "numeric" 
    ##                                                                                                                                                                                                                                                                              EXAM: x/60 (60%) 
    ##                                                                                                                                                                                                                                                                                     "numeric" 
    ##                                                                                                                                                                                                                                                        TOTAL = Coursework TOTAL + EXAM (100%) 
    ##                                                                                                                                                                                                                                                                                     "numeric" 
    ##                                                                                                                                                                                                                                                                                         GRADE 
    ##                                                                                                                                                                                                                                                                                      "factor"

``` r
glimpse(student_performance_dataset)
```

    ## Rows: 201
    ## Columns: 100
    ## $ class_group                                                                                                                                                                                                                                                                                     <fct> …
    ## $ gender                                                                                                                                                                                                                                                                                          <fct> …
    ## $ YOB                                                                                                                                                                                                                                                                                             <date> …
    ## $ regret_choosing_bi                                                                                                                                                                                                                                                                              <fct> …
    ## $ drop_bi_now                                                                                                                                                                                                                                                                                     <fct> …
    ## $ motivator                                                                                                                                                                                                                                                                                       <fct> …
    ## $ read_content_before_lecture                                                                                                                                                                                                                                                                     <fct> …
    ## $ anticipate_test_questions                                                                                                                                                                                                                                                                       <fct> …
    ## $ answer_rhetorical_questions                                                                                                                                                                                                                                                                     <fct> …
    ## $ find_terms_I_do_not_know                                                                                                                                                                                                                                                                        <fct> …
    ## $ copy_new_terms_in_reading_notebook                                                                                                                                                                                                                                                              <fct> …
    ## $ take_quizzes_and_use_results                                                                                                                                                                                                                                                                    <fct> …
    ## $ reorganise_course_outline                                                                                                                                                                                                                                                                       <fct> …
    ## $ write_down_important_points                                                                                                                                                                                                                                                                     <fct> …
    ## $ space_out_revision                                                                                                                                                                                                                                                                              <fct> …
    ## $ studying_in_study_group                                                                                                                                                                                                                                                                         <fct> …
    ## $ schedule_appointments                                                                                                                                                                                                                                                                           <fct> …
    ## $ goal_oriented                                                                                                                                                                                                                                                                                   <fct> …
    ## $ spaced_repetition                                                                                                                                                                                                                                                                               <fct> …
    ## $ testing_and_active_recall                                                                                                                                                                                                                                                                       <fct> …
    ## $ interleaving                                                                                                                                                                                                                                                                                    <fct> …
    ## $ categorizing                                                                                                                                                                                                                                                                                    <fct> …
    ## $ retrospective_timetable                                                                                                                                                                                                                                                                         <fct> …
    ## $ cornell_notes                                                                                                                                                                                                                                                                                   <fct> …
    ## $ sq3r                                                                                                                                                                                                                                                                                            <fct> …
    ## $ commute                                                                                                                                                                                                                                                                                         <fct> …
    ## $ study_time                                                                                                                                                                                                                                                                                      <fct> …
    ## $ repeats_since_Y1                                                                                                                                                                                                                                                                                <int> …
    ## $ paid_tuition                                                                                                                                                                                                                                                                                    <fct> …
    ## $ free_tuition                                                                                                                                                                                                                                                                                    <fct> …
    ## $ extra_curricular                                                                                                                                                                                                                                                                                <fct> …
    ## $ sports_extra_curricular                                                                                                                                                                                                                                                                         <fct> …
    ## $ exercise_per_week                                                                                                                                                                                                                                                                               <fct> …
    ## $ meditate                                                                                                                                                                                                                                                                                        <fct> …
    ## $ pray                                                                                                                                                                                                                                                                                            <fct> …
    ## $ internet                                                                                                                                                                                                                                                                                        <fct> …
    ## $ laptop                                                                                                                                                                                                                                                                                          <fct> …
    ## $ family_relationships                                                                                                                                                                                                                                                                            <fct> …
    ## $ friendships                                                                                                                                                                                                                                                                                     <fct> …
    ## $ romantic_relationships                                                                                                                                                                                                                                                                          <fct> …
    ## $ spiritual_wellnes                                                                                                                                                                                                                                                                               <fct> …
    ## $ financial_wellness                                                                                                                                                                                                                                                                              <fct> …
    ## $ health                                                                                                                                                                                                                                                                                          <fct> …
    ## $ day_out                                                                                                                                                                                                                                                                                         <fct> …
    ## $ night_out                                                                                                                                                                                                                                                                                       <fct> …
    ## $ alcohol_or_narcotics                                                                                                                                                                                                                                                                            <fct> …
    ## $ mentor                                                                                                                                                                                                                                                                                          <fct> …
    ## $ mentor_meetings                                                                                                                                                                                                                                                                                 <fct> …
    ## $ `A - 1. I am enjoying the subject`                                                                                                                                                                                                                                                              <dbl> …
    ## $ `A - 2. Classes start and end on time`                                                                                                                                                                                                                                                          <dbl> …
    ## $ `A - 3. The learning environment is participative, involves learning by doing and is group-based`                                                                                                                                                                                               <dbl> …
    ## $ `A - 4. The subject content is delivered according to the course outline and meets my expectations`                                                                                                                                                                                             <dbl> …
    ## $ `A - 5. The topics are clear and logically developed`                                                                                                                                                                                                                                           <dbl> …
    ## $ `A - 6. I am developing my oral and writing skills`                                                                                                                                                                                                                                             <dbl> …
    ## $ `A - 7. I am developing my reflective and critical reasoning skills`                                                                                                                                                                                                                            <dbl> …
    ## $ `A - 8. The assessment methods are assisting me to learn`                                                                                                                                                                                                                                       <dbl> …
    ## $ `A - 9. I receive relevant feedback`                                                                                                                                                                                                                                                            <dbl> …
    ## $ `A - 10. I read the recommended readings and notes`                                                                                                                                                                                                                                             <dbl> …
    ## $ `A - 11. I use the eLearning material posted`                                                                                                                                                                                                                                                   <dbl> …
    ## $ `B - 1. Concept 1 of 6: Principles of Business Intelligence and the DataOps Philosophy`                                                                                                                                                                                                         <dbl> …
    ## $ `B - 2. Concept 3 of 6: Linear Algorithms for Predictive Analytics`                                                                                                                                                                                                                             <dbl> …
    ## $ `C - 2. Quizzes at the end of each concept`                                                                                                                                                                                                                                                     <dbl> …
    ## $ `C - 3. Lab manuals that outline the steps to follow during the labs`                                                                                                                                                                                                                           <dbl> …
    ## $ `C - 4. Required lab work submissions at the end of each lab manual that outline the activity to be done on your own`                                                                                                                                                                           <dbl> …
    ## $ `C - 5. Supplementary videos to watch`                                                                                                                                                                                                                                                          <dbl> …
    ## $ `C - 6. Supplementary podcasts to listen to`                                                                                                                                                                                                                                                    <dbl> …
    ## $ `C - 7. Supplementary content to read`                                                                                                                                                                                                                                                          <dbl> …
    ## $ `C - 8. Lectures slides`                                                                                                                                                                                                                                                                        <dbl> …
    ## $ `C - 9. Lecture notes on some of the lecture slides`                                                                                                                                                                                                                                            <dbl> …
    ## $ `C - 10. The quality of the lectures given (quality measured by the breadth (the full span of knowledge of a subject) and depth (the extent to which specific topics are focused upon, amplified, and explored) of learning - NOT quality measured by how fun/comical/lively the lectures are)` <dbl> …
    ## $ `C - 11. The division of theory and practice such that most of the theory is done during the recorded online classes and most of the practice is done during the physical classes`                                                                                                              <dbl> …
    ## $ `C - 12. The recordings of online classes`                                                                                                                                                                                                                                                      <dbl> …
    ## $ `D - 1. \r\nWrite two things you like about the teaching and learning in this unit so far.`                                                                                                                                                                                                     <chr> …
    ## $ `D - 2. Write at least one recommendation to improve the teaching and learning in this unit (for the remaining weeks in the semester)`                                                                                                                                                          <chr> …
    ## $ `Average Course Evaluation Rating`                                                                                                                                                                                                                                                              <dbl> …
    ## $ `Average Level of Learning Attained Rating`                                                                                                                                                                                                                                                     <dbl> …
    ## $ `Average Pedagogical Strategy Effectiveness Rating`                                                                                                                                                                                                                                             <dbl> …
    ## $ `Project: Section 1-4: (20%) x/10`                                                                                                                                                                                                                                                              <dbl> …
    ## $ `Project: Section 5-11: (50%) x/10`                                                                                                                                                                                                                                                             <dbl> …
    ## $ `Project: Section 12: (30%) x/5`                                                                                                                                                                                                                                                                <dbl> …
    ## $ `Project: (10%): x/30 x 100 TOTAL`                                                                                                                                                                                                                                                              <dbl> …
    ## $ `Quiz 1 on Concept 1 (Introduction) x/32`                                                                                                                                                                                                                                                       <dbl> …
    ## $ `Quiz 3 on Concept 3 (Linear) x/15`                                                                                                                                                                                                                                                             <dbl> …
    ## $ `Quiz 4 on Concept 4 (Non-Linear) x/22`                                                                                                                                                                                                                                                         <dbl> …
    ## $ `Quiz 5 on Concept 5 (Dashboarding) x/10`                                                                                                                                                                                                                                                       <dbl> …
    ## $ `Quizzes and  Bonus Marks (7%): x/79 x 100 TOTAL`                                                                                                                                                                                                                                               <dbl> …
    ## $ `Lab 1 - 2.c. - (Simple Linear Regression) x/5`                                                                                                                                                                                                                                                 <dbl> …
    ## $ `Lab 2 - 2.e. -  (Linear Regression using Gradient Descent) x/5`                                                                                                                                                                                                                                <dbl> …
    ## $ `Lab 3 - 2.g. - (Logistic Regression using Gradient Descent) x/5`                                                                                                                                                                                                                               <dbl> …
    ## $ `Lab 4 - 2.h. - (Linear Discriminant Analysis) x/5`                                                                                                                                                                                                                                             <dbl> …
    ## $ `Lab 5 - Chart JS Dashboard Setup x/5`                                                                                                                                                                                                                                                          <dbl> …
    ## $ `Lab Work (7%) x/25 x 100`                                                                                                                                                                                                                                                                      <dbl> …
    ## $ `CAT 1 (8%): x/38 x 100`                                                                                                                                                                                                                                                                        <dbl> …
    ## $ `CAT 2 (8%): x/100 x 100`                                                                                                                                                                                                                                                                       <dbl> …
    ## $ `Attendance Waiver Granted: 1 = Yes, 0 = No`                                                                                                                                                                                                                                                    <fct> …
    ## $ `Absenteeism Percentage`                                                                                                                                                                                                                                                                        <dbl> …
    ## $ `Coursework TOTAL: x/40 (40%)`                                                                                                                                                                                                                                                                  <dbl> …
    ## $ `EXAM: x/60 (60%)`                                                                                                                                                                                                                                                                              <dbl> …
    ## $ `TOTAL = Coursework TOTAL + EXAM (100%)`                                                                                                                                                                                                                                                        <dbl> …
    ## $ GRADE                                                                                                                                                                                                                                                                                           <fct> …

``` the

# BEFORE
View(student_performance_dataset)

summary(student_performance_dataset)
student_performance_dataset_enj <- as.numeric(unlist(student_performance_dataset[, 49]))
hist(student_performance_dataset_enj, main = names(student_performance_dataset)[49])

model_of_the_transform <- preProcess(student_performance_dataset, method = c("scale"))
print(model_of_the_transform)
student_data_scale_transform <- predict(model_of_the_transform, student_performance_dataset)

# AFTER
summary(student_data_scale_transform)
student_performance_dataset_enj <- as.numeric(unlist(student_data_scale_transform[, 49]))
hist(student_performance_dataset_enj, main = names(student_data_scale_transform)[49])
```

``` the

summary(student_performance_dataset)
model_of_the_transform <- preProcess(student_performance_dataset, method = c("scale"))
print(model_of_the_transform)
student_data_scale_transform <- predict(model_of_the_transform, student_performance_dataset)
summary(student_data_scale_transform)
```

``` the

#BEFORE
summary(student_performance_dataset)
sapply(student_performance_dataset[, 49], sd)
model_of_the_transform <- preProcess(student_performance_dataset,
                                     method = c("scale", "center"))
print(model_of_the_transform)
student_performance_standardize_transform <- predict(model_of_the_transform, student_performance_dataset) # nolint

# AFTER
summary(student_performance_standardize_transform)
sapply(student_performance_standardize_transform[, 49], sd)
```

``` the
summary(student_performance_dataset)
model_of_the_transform <- preProcess(student_performance_dataset, method = c("range"))
print(model_of_the_transform)
student_performance_normalize_transform <- predict(model_of_the_transform, student_performance_dataset)
summary(student_performance_normalize_transform)
```

\`\`\`{Box-Cox Power Transform on the Student Performance Dataset}

# BEFORE

summary(student_performance_standardize_transform)

# Calculate the skewness before the Box-Cox transform

sapply(student_performance_standardize_transform\[, 49\], skewness, type
= 2) sapply(student_performance_standardize_transform\[, 49\], sd)

model_of_the_transform \<-
preProcess(student_performance_standardize_transform, method =
c(“BoxCox”)) print(model_of_the_transform)
student_performance_box_cox_transform \<-
predict(model_of_the_transform,
student_performance_standardize_transform)

# AFTER

summary(student_performance_box_cox_transform)

sapply(student_performance_box_cox_transform\[, 49\], skewness, type =
2) sapply(student_performance_box_cox_transform\[, 49\], sd)

# Calculate the skewness after the Box-Cox transform

sapply(student_performance_box_cox_transform\[, 49\], skewness, type =
2) sapply(student_performance_box_cox_transform\[, 49\], sd)



    ```{Yeo-Johnson Power Transform on the Student Performance Dataset}
    # BEFORE
    summary(student_performance_standardize_transform)

    # Calculate the skewness before the Yeo-Johnson transform
    sapply(student_performance_standardize_transform[, 49],  skewness, type = 2)
    sapply(student_performance_standardize_transform[, 49], sd)

    model_of_the_transform <- preProcess(student_performance_standardize_transform,
                                         method = c("YeoJohnson"))
    print(model_of_the_transform)
    student_performance_yeo_johnson_transform <- predict(model_of_the_transform, # nolint
                                               student_performance_standardize_transform)

    # AFTER
    summary(student_performance_yeo_johnson_transform)

``` pca

summary(student_performance_dataset)

model_of_the_transform <- preProcess(student_performance_dataset,
                                     method = c("scale", "center", "pca"))
print(model_of_the_transform)
student_performance_dataset_pca_dr <- predict(model_of_the_transform, student_performance_dataset)

summary(student_performance_dataset_pca_dr)
dim(student_performance_dataset_pca_dr)
```
