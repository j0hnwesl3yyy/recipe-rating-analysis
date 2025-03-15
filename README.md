# Macronutrients & Ratings: The Impact of Carbs and Protein on Recipe Ratings

Authors: Ananya Krishnan and JohnWesley Pabalate 

## Introduction
Food and nutrition play a significant role in people’s choices when selecting recipes. Some individuals may prefer high-carb meals, while others prioritize protein-rich options. But do these macronutrient differences influence how people rate recipes?

In this project, we will investigate whether high-carb, low-protein recipes receive significantly different ratings compared to other recipes.

### 🤔 Why Does This Matter?
Understanding how carbohydrates and protein impact recipe ratings can provide valuable insights for different groups:

1. 🍽️ Home Cooks & Food Influencers – Knowing which types of recipes get higher ratings can help in creating meals that people love. If high-carb, low-protein recipes are rated lower, food bloggers might adjust their ingredient choices to appeal to a wider audience.

2. 🏪 Food Chains & Restaurants – Businesses can use this information to develop menus and packaged foods that match customer preferences. If people favor certain macronutrient combinations, companies can tailor their products to boost customer satisfaction.

3. 🥦 Nutritionists & Dietitians – Understanding how people perceive high-carb, low-protein meals can provide insights into eating habits and public attitudes toward nutrition. This can help in designing dietary recommendations that are both healthy and well-received.

The first dataframe (df) `recipes` was extracted from the **RAW_recipes.csv** file and it contains 83782 rows along with these columns listed below:

| Column             | Description                                                                                                                                                                                       |
| :----------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `'name'`           | Recipe name                                                                                                                                                                                       |
| `'id'`             | Recipe ID                                                                                                                                                                                         |
| `'minutes'`        | Minutes to prepare recipe                                                                                                                                                                         |
| `'contributor_id'` | User ID who submitted this recipe                                                                                                                                                                 |
| `'submitted'`      | Date recipe was submitted                                                                                                                                                                         |
| `'tags'`           | Food.com tags for recipe                                                                                                                                                                          |
| `'nutrition'`      | Nutrition information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV stands for “percentage of daily value” |
| `'n_steps'`        | Number of steps in recipe                                                                                                                                                                         |
| `'steps'`          | Text for recipe steps, in order                                                                                                                                                                   |
| `'description'`    | User-provided description                                                                                                                                                                         |
| `'ingredients'`    | Text for recipe ingredients                                                                                                                                                                       |
| `'n_ingredients'`  | Number of ingredients in recipe                                                                                                                                                                   |

The first dataframe (df) `reviews` was extracted from the **RAW_interaction.csv** file and it contains 731927 rows along with these columns listed below:

| Column        | Description         |
| :------------ | :------------------ |
| `'user_id'`   | User ID             |
| `'recipe_id'` | Recipe ID           |
| `'date'`      | Date of interaction |
| `'rating'`    | Rating given        |
| `'review'`    | Review text         |

The key columns relevant to our analysis include:

- `recipe_id` and `id` – A unique identifier for each recipe.
- `nutrition` – A list containing nutritional values such as calories, fat, sugar, sodium, protein, carbohydrates, and more.
- `rating` – The user rating given to the recipe.
  
Overall, by analyzing recipe ratings, we can find trends that help people make better food choices, guide businesses in developing better products, and improve our understanding of how nutrition influences preferences!

## Data Cleaning and Exploratory Data Analysis

For data cleaning, to make our analysis more efficient and valid to use, we did the following: 

1. **Left merge the `recipes` and `reviews` df left_on = 'id' and right_on = 'recipe_id'.**

   - From this merge, we were able to match the recipe from both separate dataframes, and having one big dataframe with 234429 rows that has both the recipe information and the ratings.
     

   - | Column             | Description |
     | :----------------- | :---------- |
     | `'name'`           | object      |
     | `'id'`             | int64       |
     | `'minutes'`        | int64       |
     | `'contributor_id'` | int64       |
     | `'submitted'`      | object      |
     | `'tags'`           | object      |
     | `'nutrition'`      | object      |
     | `'n_steps'`        | int64       |
     | `'steps'`          | object      |
     | `'description'`    | object      |
     | `'ingredients'`    | object      |
     | `'n_ingredients'`  | int64       |
     | `'user_id'`        | float64     |
     | `'recipe_id'`      | float64     |
     | `'date'`           | object      |
     | `'rating'`         | float64     |
     | `'review'`         | object      |
     


2. **Fill all ratings of 0 with np.nan.**

  - Rating is scaled from 1 to 5, 1 meaning the lowest rating while 5 means the highest rating. We replace all 0s in the ratings column with NaN values. The 0 represents no rating given, but it will influence any calculations we perform with the ratings.


3. **Calculate the average ratings for each recipe and store it in avg_recipe_ratings**

   - Merges the recipe_ratings dataframe with the avg_recipe_rating dataset to include the average rating of each recipe, creating a column for the average rating.
   - For the purposes of analyzing the missingness and baseline, created a copy of the recipe_rating dataframe.
     

4. **Only wanted certain columns, so we only retrieved `id` (renamed to `recipe_id`), `rating`, `avg_rating`, and `nutrition`; dropping the other columns.**
   - |  Column            | Description |
     | :----------------- | :---------- |
     | `'recipe_id'`      | int64       |
     | `'rating'`         | float64     |
     | `'avg_rating'`     | float64     |
     | `'nutrition'`      | object      |


       
5. **Observed that the `nutrition` column contains string format, so we converted so it becomes a list.**

   - Separated each value in the list [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)] to its category (column) by indexing the `nutrition` column.

6. **Quantile based method to get high carbs and low protein** 


#### Result
| recipe_id | rating | avg_rating | calories | total_fat | sugar | sodium | protein | saturated_fat | carbohydrates | carb_prop | protein_prop | high_carb_low_protein | shuffled_rating |
|-----------|--------|------------|----------|-----------|-------|--------|---------|---------------|--------------|-----------|--------------|----------------------|----------------|
| 333281    | 4.0    | 4.0        | 138.4    | 10.0      | 50.0  | 3.0    | 3.0     | 19.0          | 6.0          | 0.173410  | 0.086705     | False                | 4.461538       |
| 453467    | 5.0    | 5.0        | 595.1    | 46.0      | 211.0 | 22.0   | 13.0    | 51.0          | 26.0         | 0.174761  | 0.087380     | False                | 5.000000       |
| 306168    | 5.0    | 5.0        | 194.8    | 20.0      | 6.0   | 32.0   | 22.0    | 36.0          | 3.0          | 0.061602  | 0.451745     | False                | 4.916667       |
| 306168    | 5.0    | 5.0        | 194.8    | 20.0      | 6.0   | 32.0   | 22.0    | 36.0          | 3.0          | 0.061602  | 0.451745     | False                | 5.000000       |
| 306168    | 5.0    | 5.0        | 194.8    | 20.0      | 6.0   | 32.0   | 22.0    | 36.0          | 3.0          | 0.061602  | 0.451745     | False                | 4.777778       |


### Univariate Analysis

<iframe
  src="assets/dist_avg_rating_histogram.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
With the **distribution of average** recipe ratings, with the majority of ratings concentrated at the higher end, particularly around 5. Since this histogram is skewed to the left, it suggests that most recipes receive very positive ratings, while lower ratings are significantly less common.


<iframe
  src="assets/dist_carb_prop_histogram.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
With the **distribution of carbohydrate proportions** in recipes, with most recipes having a relatively low carbohydrate proportion, approximately between 0 and 0.3. The peak around 0.1–0.2 suggests that a significant number of recipes contain moderate carbohydrate content, and higher proportions are minimally present.

<iframe
  src="assets/dist_protein_prop_histogram.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
With the **distribution of protein proportions** in recipes, with most values concentrated between 0.1 and 0.3. The distribution is right skewed, meaning there are fewer recipes with very high protein proportions, but a small number extend towards the higher end.

<iframe
  src="assets/dist_highcarb_lowprotein_histogram.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
With the **distribution of recipes classified as high-carb and low-protein**. The majority of recipes do not fall into this category, while only a small proportion are considered high-carb and low-protein, indicating that most recipes have a more balanced or different macronutrient composition.

### Bivariate Analysis

<iframe
  src="assets/carb_corp_heat.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
This heatmap shows that most recipes have a low carbohydrate proportion and tend to receive high ratings (around 4 to 5 stars). Recipes with higher carbohydrate proportions are less common across all ratings, but the moderate level of carbohydrate proportions is more saturated with 5 star ratings.

<iframe
  src="assets/protein_prop_heat.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
This heatmap shows that most recipes have a low to moderate protein proportion and tend to receive high ratings (around 4 to 5 stars). Recipes with very high protein proportions are less common across all rating levels, while low protein proportions tend to be more saturated with 5 star ratings.

### Interesting Aggregates


## Assessment of Missingness

Three columns, `'date'`, `'rating'`, and `'review'`, in the merged dataset have a significant amount of missing values, so we decided to assess the missingness on the dataframe.


### NMAR Analysis & Missingness Dependency
After merging the two dataframes together, we were left with three columns with an abundant amount of missing values. Those three columns are `rating`, `review`, `descriptions`. Among the three columns with missing values—**`description` (114 missing), `rating` (15,035 missing), and `review` (57 missing)**, the most likely **NMAR (Not Missing at Random)** column is **`rating`**. Missingness in `rating` is substantial, and a key reason for this could be that users choose whether or not to rate a recipe based on their experience. If users only leave ratings when they strongly like or dislike a recipe, the missing ratings are likely **not missing randomly**, but rather **dependent on the rating itself** (which we cannot observe for missing cases). This makes `rating` a strong NMAR candidate because the likelihood of missingness is tied to the unrecorded rating values, meaning that missingness itself carries implicit information about user sentiment. **`description` could also be NMAR**, as some users may deliberately choose not to write a description, possibly due to lack of effort or because they believe it is unnecessary, though this is more speculative. On the other hand, **`review` is less likely NMAR** because the missingness of reviews is probably influenced by external factors such as user engagement levels or the rating system itself, making it more **MAR (Missing at Random)** rather than NMAR. Since the missingness in `review` does not necessarily depend on the content of the review itself, it is less indicative of NMAR behavior. Ultimately, **`rating` is the strongest NMAR candidate because its absence likely depends on the users' choice to rate, which is influenced by their personal experience with the recipe—something that is unobserved when the rating is missing.**

<iframe
  src="assets/rating_review_missing_bar_chart.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<iframe
  src="assets/review_rating_kde.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<iframe
  src="assets/tvd1_distribution.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
**np.mean(np.array(tvds) >= observed_tvd) = np.float64(0.928)**
The p-value (0.928) indicates that the observed TVD is not significantly different from what we’d expect under random chance. This suggests that review missingness is not dependent on rating. The distribution of missing vs. non-missing reviews appears random, with no strong pattern linked to rating values. Therefore, we fail to reject the null hypothesis, meaning review missingness does not systematically vary based on rating.

<iframe
  src="assets/n_steps_kde.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<iframe
  src="assets/nsteps_review_missing_bar_chart.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<iframe
  src="assets/tvd2_distribution.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

## Hypothesis Testing

Our goal is to see if carbohydrate and protein content affect ratings of recipes. We define high-carb, low-protein recipes as those that fall into both:

The top 25th percentile for the proportion of calories from carbohydrates.
The bottom 25th percentile for the proportion of calories from protein.

**Null Hypothesis (H₀):** Recipes with high carb % and low protein % receive the same ratings as other recipes.

**Alternative Hypothesis (Hₐ):** Recipes with high carb % and low protein % receive significantly different ratings.

**Test statistic:** Mean difference in ratings between the high-carb, low-protein group and others.

**Significance level:** 0.05

**With every run, the p-value (0.01) is always less than the significance level 0.05, so we will reject the null hypothesis.**

**Justification:** Making it robust. The mean rating difference is a simple, clear measure that directly shows if high-carb, low-protein recipes get better or worse ratings than other recipes. Using the 25th percentiles gives us enough recipes in each group to make a fair comparison without cherry-picking data. The standard 0.05 significance level follows established scientific practice, and getting a p-value of 0.01 gives us strong confidence that our findings aren't just due to random chance. This approach ensures our conclusion about how carbs and protein affect recipe ratings is reliable and trustworthy.

## Framing a Prediction Problem

We plan to **predict average rating of a recipe** which would be a **classification problem** since we can treat rating as a ordinal categorical variable if we round the average rating so that we only have [1, 2, 3, 4, 5] as possible values. To address our prediction problem, we will build a multi-class classifier since our average ratings have 5 possible values that the model will predict from.

We chose the average rating of a recipe as a response variable because it is a good representation of the overall rating of a recipe. We have also previously found significant correlation between rating and sugary recipes, which are recipes with proportion of sugar higher than the average proportion of sugar, so we may be able to predict the rating through the proportion of sugar.

To evaluate our model, we will use the f1 score instead of accuracy, because the distribution for the ratings are heavily skewed left with most ratings concentrated in the higher ratings (4-5). This means that there are more recipes with higher average ratings. If we use accuracy, the model's performance may be misleading due to the imbalanced classes.

The information we have prior making our prediction are all the columns in the `rating` dataset, which are listed in the introduction section. All those columns are features relating to the recipes themselves, thus we would have access to it even though no one has made a rating and review on them.

## Baseline Model

For our baseline model, we are utilizing a random forest classifier and split the data points into training and test sets. The features we are using for this model is `'prop_sugar'`, a column containing quantitative numerical values, and `'is_dessert'`, a column containing nominal values since they are boolean values.

We one hot encoded the boolean values in `'is_dessert'` with the corresponding 0 and 1 values and dropped one of the encoded columns. This step allows us to train the model appropriately.

The metric, **F1 score**, of this model is **0.87**. The F1 score for each rating categoires are 0.20, 0.47, 0.50, 0.74, and 0.92 for rating of 1s, 2s, 3s, 4s, and 5s respectively. The metrics let us know that the model predicts better for rating of 4s and 5s and not as accurate for the lower ratings. The reason for this could be that there are more recipes with rating 4s and 5s in the dataset compared to other ratings. With the more data points, the model predicted better for higher ratings.

## Final Model

For the final model, we used `'is_dessert'`, `'minutes'`, `'calories (#)'`, `'submitted'`, and `'prop_sugar'` as the features.

`'is_dessert'`

The column categorizes the data as dessert or not dessert by checking if the recipe's tags contain 'dessert'. We chose this feature because based on the the bar graph we construsted between `'average rating'` and `'is_dessert'`, we saw that for the higher ratings (4 and 5) there are less dessert recipes. This trend might be useful in helping the model predict the average rating of a recipe. We one hot encoded this column like we did for the baseline model.

`'minutes'`

The column is the cooking time of the recipe in minutes. By constructing a bivariate table of the `'minutes'` and `'average rating'`, we learned that the recipes took longer to cook tend to have medicore ratings, like ratings of 2 or 3. The differences between the mean minutes among the different ratings makes us believe that it could help with our predicition model. It is also reasonable that a recipe that takes long to make would lead to lower rating since people are busy nowadays and lack patience. We used `StandardScaler` to standardize the `'minutes'` feature to guarantee that the cooking time are in a comparable range since some recipes has extremely long cooking times.

`'calories (#)'`

The column contains the total calories of the recipe. By constructing a bivariate table of the `'calories (#)'` and `'average rating'`, we learned that recipes with higher rating typically has less calories. A lower calories generally indicates that the recipe is healthier, thus it is logical that it has a higher rating. Knowing this, we think the relationship between `'calories (#)'` and `'average rating'` would help our model predict better. To transform the `'calories (#)'` feature, we used `RobustScaler`, which scales the numerical features while handling outliers effectively. From the EDA, we learned that the columns contains many outliers, and it might introduces bias to our model, so we engineered the feature to minimize the ourlier effects.

`'submitted'`

The column contains information of the date that the recipe was submitted. In data cleaning process, we converted the column to be `datetime[ns]` and now we pulled out only the year using `FunctionTransformer`. When we created a table of `'submitted'` and `'average rating'`, we noticed that recipes submitted in recent years has a lower ratings. This could be due to the lack of novelity of newer recipes since most of the classic recipes might already posted on the website. The trend between `'submitted'` and `'average rating'` could be also useful in improving our model.

`'prop_sugar'`

As mentioned numerous time in earlier sections, this column contains information of the proportion of sugar in calories out of the total calories of the recipe. According to our hypothesis testing, people seems more likely to rate a sugary recipe lower than recipes that are not sugary. This takeaway motivates us use this feature since the relationship could be a significant deciding factor when making predicition on the `'average rating'`. For this feature, we will leave the column as it is.

We used `RandomForestClassifier` as our modeling algorithm and conducted `GridSearchCV` to tune the hyperparameters of `max_depth` and `n_estimators` of the `RandomForestClassifier`. Decision trees are prone to high variance, and the two hyperparameters we chose serve a way to control the variance and avoid overfitting the training set. The best combination of the hyperparameters is 42 for the `max_depth` and 142 for the `n_estimators`.

The metric, **F1 Score**, of the final model is **0.92**, which is a 0.05 increase from the F1 Score of the baseline model. Moreover, the F1 score of each of the rating also improved. The F1 score for each rating categoires are now 0.36, 0.66, 0.68, 0.85, and 0.95 for rating of 1s, 2s, 3s, 4s, and 5s respectively.

## Fairness Analysis

For our fairness analysis, we split the recipes into two groups: high calories and low calories. We designated high calorie recipes to be ones with calories > 301.1 and low calorie recipes to be ones with calories <= 301.1. We found that the median calories for our data set is **301.1** which is why we chose it as the threshold. We used median instead of mean, because we previously found that calories had many high outliers which can skew our results. We chose to evaluate the **precision parity** of the model for the two groups, because we think it’s more important for the model to correctly identify the rating of a recipe among all instances of that rating. False positives would not be good since it would mislead users with the incorrectly labeled ratings. False positives would not be good since it would mislead users with the incorrectly labeled ratings. For example, if we predicted recipes with lower calories to have a bad rating, people would be discouraged to try them. For recipes with lower calories, it wouldn’t be good to mislabel them, as low calorie recipes may be healthier for people.

**Null Hypothesis**: Our model is fair. Its precision for recipes with higher calories and lower calories are roughly the same, and any differences are due to random chance.

**Alternative Hypothesis**: Our model is unfair. Its precision for recipes with lower calories is lower than its precision for recipes with higher calories.

**Test Statistic**: Difference in precision (low calories - high calories)

**Significance Level**: 0.05

<iframe
  src="assets/empirical_precision.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

To run the permutation test, we created a new column `is_high_calories` to differentiate between the low and high calorie recipes. When we took the difference in their precision, we got an observed test statistic of **-0.023**. We shuffled the `is_high_calories` column for 1000 times to collect 1000 simulating differences in the two distributions as described in the test statistic. After running our permutation test, we got a p-value of **0.0**. Since the p-value of 0.0 is less than 0.05, we reject the null hypothesis that our model is fair. The model's precision for recipes with lower calories is lower than its precision for recipes with higher calories.
