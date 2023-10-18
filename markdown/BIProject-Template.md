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
```

``` student

dim(student_performance_dataset)
```

``` student
sapply(student_performance_dataset, class)
glimpse(student_performance_dataset)
```

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

``` histogram

#BEFORE
hist(student_performance_dataset_enj, main = names(student_performance_dataset)[49])


#AFTER
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
