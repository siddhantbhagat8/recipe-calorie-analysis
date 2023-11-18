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