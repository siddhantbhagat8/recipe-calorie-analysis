# recipe-calorie-analysis

## Introduction
My analysis seeks to unravel a compelling query: **"Do recipes with a higher number of ingredients typically contain more calories?"** This investigation delves into the intriguing relationship between the complexity of a recipe, in terms of ingredient count, and its caloric content. This question is crucial for readers who are health-conscious, culinary enthusiasts, or anyone interested in understanding the nutritional implications of their food choices. By exploring our dataset, readers can gain insights into how the simplicity or complexity of recipes influences their calorie count, thereby aiding in making more informed dietary decisions. This study not only caters to those tracking their caloric intake but also offers valuable knowledge for culinary professionals and hobbyists who are keen to balance flavor and nutrition.

## Datasets
There are primarily two datasets that have been used in my analysis: `recipes` and `ratings`.
Description of `recipes` dataset:
![Recipes Description](assets/recipes_description.png)
The `recipes` dataset had **83782 rows** and **12 columns**
Description of `ratings` dataset:
![Ratings Description](assets/ratings_description.png)
The `ratings` dataset had **731927 rows** and **5 columns**

## Cleaning and EDA
The following steps were taken to clean the above two datasets for exploratory analysis:
1. Left merged the `recipes` and `ratings` datasets on the id of recipes
2. In the `merged` dataset, filled all ratings of 0 with `np.nan`. This is because recipes with 0 ratings indicate missing values as the lowest someone can give a recipe a rating on a scale of 5 is 1. So, 0 represents missing values
3. Added a column `rating_average` to the `recipes` dataset which gives the average rating for a recipe 
4. Added a column `calories` to the `recipes` dataset by extracting the calories from the column `nutrition` which contained many values
5. Changed the type of `submitted` from `object` to `datetime` to better represent the date and access the years for future use. Did the same for the `submitted` column in the `merged` dataset
6. Added a column `ingr_intensive` to the `recipes dataframe` if `n_ingredients` is greater than 12 or not

First few rows of the `merged` dataset with important columns:

| name                                 |     id |   n_ingredients |   n_steps | nutrition                                    | ingredients                                                                                                                                                                    | submitted           |   rating |
|--------------------------------------|--------|-----------------|-----------|----------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------|----------|
| 1 brownies in the world    best ever | 333281 |               9 |        10 | [138.4, 10.0, 50.0, 3.0, 3.0, 19.0, 6.0]     | ['bittersweet chocolate', 'unsalted butter', 'eggs', 'granulated sugar', 'unsweetened cocoa powder', 'vanilla extract', 'brewed espresso', 'kosher salt', 'all-purpose flour'] | 2008-10-27 00:00:00 |        4 |
| 1 in canada chocolate chip cookies   | 453467 |              11 |        12 | [595.1, 46.0, 211.0, 22.0, 13.0, 51.0, 26.0] | ['white sugar', 'brown sugar', 'salt', 'margarine', 'eggs', 'vanilla', 'water', 'all-purpose flour', 'whole wheat flour', 'baking soda', 'chocolate chips']                    | 2011-04-11 00:00:00 |        5 |
| 412 broccoli casserole               | 306168 |               9 |         6 | [194.8, 20.0, 6.0, 32.0, 22.0, 36.0, 3.0]    | ['frozen broccoli cuts', 'cream of chicken soup', 'sharp cheddar cheese', 'garlic powder', 'ground black pepper', 'salt', 'milk', 'soy sauce', 'french-fried onions']          | 2008-05-30 00:00:00 |        5 |
| 412 broccoli casserole               | 306168 |               9 |         6 | [194.8, 20.0, 6.0, 32.0, 22.0, 36.0, 3.0]    | ['frozen broccoli cuts', 'cream of chicken soup', 'sharp cheddar cheese', 'garlic powder', 'ground black pepper', 'salt', 'milk', 'soy sauce', 'french-fried onions']          | 2008-05-30 00:00:00 |        5 |
| 412 broccoli casserole               | 306168 |               9 |         6 | [194.8, 20.0, 6.0, 32.0, 22.0, 36.0, 3.0]    | ['frozen broccoli cuts', 'cream of chicken soup', 'sharp cheddar cheese', 'garlic powder', 'ground black pepper', 'salt', 'milk', 'soy sauce', 'french-fried onions']          | 2008-05-30 00:00:00 |        5 |

First few rows of `recipes` dataset with important columns:

|     id |   n_ingredients |   calories | ingr_intensive   | submitted           |
|--------|-----------------|------------|------------------|---------------------|
| 333281 |               9 |      138.4 | False            | 2008-10-27 00:00:00 |
| 453467 |              11 |      595.1 | False            | 2011-04-11 00:00:00 |
| 306168 |               9 |      194.8 | False            | 2008-05-30 00:00:00 |
| 286009 |               7 |      878.3 | False            | 2008-02-12 00:00:00 |
| 475785 |              13 |      267   | True             | 2012-03-06 00:00:00 |

## Univariate Analysis
The following is the distribution of the number of ingredients across all recipes. The distribution appears to be roughly unimodal, peaking at 8 ingredients, suggesting that most of the recipes contain ingredients within this range. Thereafter, the count of recipes gradually decreases as the number of ingredients increases, showing that recipes with a very high number of ingredients are less common. The plot also appears to be right skewed
<iframe src="assets/dist_n_ingr.html" width=800 height=600 frameBorder=0></iframe>

The following is the distribution of the number of calories in a collection of recipes. The x-axis has been restricted to 4000 calories to eliminate extreme outliers in the dataset. The distribution shows a right-skewed pattern, indicating that most recipes have a lower calorie count, with the frequency decreasing as the calorie count increases. This suggests that recipes with very high-calorie counts are less common. The majority of the recipes seem to cluster under 500 calories and peak between 150-169.9 calories, which may indicate a tendency towards lighter or more health-conscious recipes in the dataset.
<iframe src="assets/dist_calories.html" width=800 height=600 frameBorder=0></iframe>

## Bivariate Analysis
The following is a scatter plot visualizing the relationship between the number of ingredients in a recipe (shown on the x-axis) and the calorie content of that recipe (shown on the y-axis). Each dot represents an individual recipe, with its position determined by the number of ingredients and its caloric value. From the plot, it appears that there is a wide distribution of calorie values across recipes with varying numbers of ingredients. The recipes seem to be more concentrated in the lower range of ingredients, with fewer recipes having a high number of ingredients
<iframe src="assets/bivariate.html" width=800 height=600 frameBorder=0></iframe>

## Interesting Aggregrates
This pivot table shows the trend of average number of ingredients and average calories per recipe from 2008-2018. It is observed that the average ingredients increase slightly by the year whereas the average calories show a linear increase towards 2018

|   year |   total_recipes |   avg_n_ingredients |   avg_calories |   avg_rating |
|-------:|----------------:|--------------------:|---------------:|-------------:|
|   2008 |           30745 |             8.99382 |        418.853 |      4.59867 |
|   2009 |           22547 |             9.23715 |        427.918 |      4.62367 |
|   2010 |           11902 |             9.24668 |        416.971 |      4.64025 |
|   2011 |            7573 |             9.26859 |        454.809 |      4.6777  |
|   2012 |            5187 |             9.58974 |        425.329 |      4.66959 |
|   2013 |            3792 |             9.6875  |        456.576 |      4.65719 |
|   2014 |            1049 |             9.6387  |        478.594 |      4.60534 |
|   2015 |             306 |            10.4935  |        452.436 |      4.69137 |
|   2016 |             204 |             9.60294 |        538.805 |      4.52638 |
|   2017 |             288 |            10.0139  |        743.519 |      4.47463 |
|   2018 |             189 |            12.1534  |        979.432 |      4.60891 |

The following is a line to show the linear increase in average calories per year
<iframe src="assets/grouped_year.html" width=800 height=600 frameBorder=0></iframe>

This pivot table shows the trend of average calories per the number of ingredients in a recipe as well as the total number of recipes per the number of ingredients. It is also interesting to see the maximum amount of calories a dish can have with a certain number of ingredients!

|   n_ingredients |   total_recipes |   avg_calories |   min_calories |   max_calories |   avg_rating |
|----------------:|----------------:|---------------:|---------------:|---------------:|-------------:|
|               1 |              14 |        714.65  |            7.8 |         3590.2 |      4.86154 |
|               2 |             747 |        331.983 |            0   |        12135.5 |      4.69258 |
|               3 |            2342 |        306.112 |            0   |        13101.5 |      4.66203 |
|               4 |            4481 |        336.009 |            0   |        45609   |      4.63394 |
|               5 |            6580 |        347.801 |            0   |        17551.6 |      4.64743 |
|               6 |            7524 |        355.885 |            0   |        17554   |      4.6331  |
|               7 |            8515 |        391.13  |            0.7 |         8300.5 |      4.62407 |
|               8 |            8923 |        387.032 |            1   |         9702.6 |      4.61152 |
|               9 |            8628 |        422.505 |            0.3 |        17280.4 |      4.60638 |
|              10 |            8033 |        449.816 |            0.7 |        18268.7 |      4.61023 |

The following line plot shows the trend in number of recipes and average calories per number of ingredients
<iframe src="assets/grouped_ingr.html" width=800 height=600 frameBorder=0></iframe>

## Assessment of Missingness

### NMAR analysis
For the NMAR analysis, I looked at the missing values in the `review` column in the merged dataframe. This column is likely NMAR because the the reviews are likely missing due to the value itself. People leave a review for the recipe either because they loved it and want to express gratitude or praise for the shared recipe or they hated it as it didn't turn out the way they expected it too. In most cases, people remain unaffected by the recipe and do not bother giving it a review. This makes the `review` it NMAR.
The `review` column can be made MAR by introducing another column in the dataframe that is indicative of a person's happiness with a smiley or frown face or a neutral emoji that can can tell why the person did not review the recipe. This would make it MAR.

### Misingness Dependency

#### Calories and Rating
**Null Hypothesis:** There is no difference between the mean calories of recipes with missing ratings and those with ratings 
**Alternate Hypothesis:** There is a difference between the mean calories of recipes with and without ratings.
**Test Statistic:** Absolute difference in the mean calories of the two groups. We choose this test statistic because `calories` and `ratings` are both numerical variables

The selective dataframe in use has a `rating_missing` column to help conduct the test:
|   calories |   rating | rating_missing   |
|-----------:|---------:|:-----------------|
|      138.4 |        4 | False            |
|      595.1 |        5 | False            |
|      194.8 |        5 | False            |
|      194.8 |        5 | False            |
|      194.8 |        5 | False            |

The distribution of `calories` when `rating` is missing and when `rating` is present appears to be skewed to the right
<iframe src="assets/cr1.html" width=800 height=600 frameBorder=0></iframe>

We perform a permutation test by randomly shuffling the `rating_missing` column a 1000 times and also record the observed test statistic (marked in the red line) which is 69.01
<iframe src="assets/cr2.html" width=800 height=600 frameBorder=0></iframe>

The p-value from this test was nearly 0.0 which means that we __reject the null hypothesis__ and conclude that missingness of `ratings` __depends__ on `calories`

#### Minutes and Rating
**Null Hypothesis:** There is no difference between the mean minutes of recipes with missing ratings and those with ratings 
**Alternate Hypothesis:** There is a difference between the mean minutes of recipes with and without ratings.
**Test Statistic:** Absolute difference in the mean minutes of the two groups. We choose this test statistic because `minutes` and `ratings` are both numerical variables

The selective dataframe in use has a `rating_missing` column to help conduct the test:
|   minutes |   rating | rating_missing   |
|----------:|---------:|:-----------------|
|        40 |        4 | False            |
|        45 |        5 | False            |
|        40 |        5 | False            |
|        40 |        5 | False            |
|        40 |        5 | False            |

The distribution of `minutes` when `rating` is missing and when `rating` is present appears to be skewed to the right
<iframe src="assets/mr1.html" width=800 height=600 frameBorder=0></iframe>

We perform a permutation test by randomly shuffling the `rating_missing` column a 1000 times and also record the observed test statistic (marked in the red line) which is 51.45
<iframe src="assets/mr2.html" width=800 height=600 frameBorder=0></iframe>

The p-value from this test was 0.102 which means that we __fail to reject the null hypothesis__ and conclude that missingness of `ratings` __does not depend__ on `calories`

## Hypothesis Test