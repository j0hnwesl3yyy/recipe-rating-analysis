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
**recipe_ratings**
| recipe_id | rating | avg_rating | calories | total_fat | sugar | sodium | protein | saturated_fat | carbohydrates | carb_prop | protein_prop | high_carb_low_protein | shuffled_rating |
|-----------|--------|------------|----------|-----------|-------|--------|---------|---------------|--------------|-----------|--------------|----------------------|----------------|
| 333281    | 4.0    | 4.0        | 138.4    | 10.0      | 50.0  | 3.0    | 3.0     | 19.0          | 6.0          | 0.173410  | 0.086705     | False                | 4.461538       |
| 453467    | 5.0    | 5.0        | 595.1    | 46.0      | 211.0 | 22.0   | 13.0    | 51.0          | 26.0         | 0.174761  | 0.087380     | False                | 5.000000       |
| 306168    | 5.0    | 5.0        | 194.8    | 20.0      | 6.0   | 32.0   | 22.0    | 36.0          | 3.0          | 0.061602  | 0.451745     | False                | 4.916667       |
| 306168    | 5.0    | 5.0        | 194.8    | 20.0      | 6.0   | 32.0   | 22.0    | 36.0          | 3.0          | 0.061602  | 0.451745     | False                | 5.000000       |
| 306168    | 5.0    | 5.0        | 194.8    | 20.0      | 6.0   | 32.0   | 22.0    | 36.0          | 3.0          | 0.061602  | 0.451745     | False                | 4.777778       |

**merged_df**
| name                                       | id      | minutes | contributor_id | submitted   | tags                                                                 | nutrition                             | n_steps | steps                                                 | description                                   | ingredients                                         | n_ingredients | user_id  | recipe_id | date        | rating | review                                             | avg_rating |
|--------------------------------------------|---------|---------|---------------|-------------|----------------------------------------------------------------------|----------------------------------------|---------|-----------------------------------------------------|-----------------------------------------------|--------------------------------------------------|--------------|----------|-----------|-------------|--------|-----------------------------------------------------|------------|
| 1 brownies in the world best ever         | 333281  | 40      | 985201        | 2008-10-27  | ['60-minutes-or-less', 'time-to-make', 'course...']                  | [138.4, 10.0, 50.0, 3.0, 3.0, 19.0, 6.0]  | 10      | ['heat the oven to 350f and arrange the rack i...'] | these are the most; chocolatey, moist, rich, d... | ['bittersweet chocolate', 'unsalted butter', ...] | 9            | 386585.0 | 333281.0  | 2008-11-19  | 4.0    | These were pretty good, but took forever to ba... | 4.0        |
| 1 in canada chocolate chip cookies        | 453467  | 45      | 1848091       | 2011-04-11  | ['60-minutes-or-less', 'time-to-make', 'cuisine...']                  | [595.1, 46.0, 211.0, 22.0, 13.0, 51.0, 26.0] | 12      | ['pre-heat oven the 350 degrees f', 'in a mixi...'] | this is the recipe that we use at my school ca... | ['white sugar', 'brown sugar', 'salt', 'margar...'] | 11           | 424680.0 | 453467.0  | 2012-01-26  | 5.0    | Originally I was gonna cut the recipe in half ... | 5.0        |
| 412 broccoli casserole                    | 306168  | 40      | 50969         | 2008-05-30  | ['60-minutes-or-less', 'time-to-make', 'course...']                  | [194.8, 20.0, 6.0, 32.0, 22.0, 36.0, 3.0] | 6       | ['preheat oven to 350 degrees', 'spray a 2 qua...'] | since there are already 411 recipes for brocco... | ['frozen broccoli cuts', 'cream of chicken sou...'] | 9            | 29782.0  | 306168.0  | 2008-12-31  | 5.0    | This was one of the best broccoli casseroles t... | 5.0        |
| 412 broccoli casserole                    | 306168  | 40      | 50969         | 2008-05-30  | ['60-minutes-or-less', 'time-to-make', 'course...']                  | [194.8, 20.0, 6.0, 32.0, 22.0, 36.0, 3.0] | 6       | ['preheat oven to 350 degrees', 'spray a 2 qua...'] | since there are already 411 recipes for brocco... | ['frozen broccoli cuts', 'cream of chicken sou...'] | 9            | 1196280.0 | 306168.0  | 2009-04-13  | 5.0    | I made this for my son's first birthday party ... | 5.0        |
| 412 broccoli casserole                    | 306168  | 40      | 50969         | 2008-05-30  | ['60-minutes-or-less', 'time-to-make', 'course...']                  | [194.8, 20.0, 6.0, 32.0, 22.0, 36.0, 3.0] | 6       | ['preheat oven to 350 degrees', 'spray a 2 qua...'] | since there are already 411 recipes for brocco... | ['frozen broccoli cuts', 'cream of chicken sou...'] | 9            | 768828.0 | 306168.0  | 2013-08-02  | 5.0    | Loved this. Be sure to completely thaw the br... | 5.0        |

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
After merging the two dataframes together, we were left with three columns with an abundant amount of missing values. Those three columns are `rating`, `review`, and `description`. Among the three columns with missing values `description` (114 missing), `rating` (15,035 missing), and `review` (57 missing), the most likely NMAR (Not Missing at Random) column is review.

Missingness in review is significant, and a key reason for this could be that users choose whether or not to leave a review based on their experience with the recipe. If users only write reviews when they feel strongly about a recipe—either positively or negatively—then the missing reviews are likely not missing randomly, but rather dependent on the sentiment of the review itself (which we cannot observe for missing cases). This makes review a strong NMAR candidate because the likelihood of missingness is tied to unrecorded user opinions, meaning that the absence of a review may carry implicit information about user satisfaction or dissatisfaction.

description could also be NMAR, as some users may deliberately choose not to write a description, possibly due to lack of effort or because they believe it is unnecessary, though this is more speculative. On the other hand, rating is less likely NMAR because the missingness of ratings is probably influenced by external factors such as whether a user engaged with the rating system at all, making it more MAR (Missing at Random) rather than NMAR. Since the missingness in rating does not necessarily depend on the content of the rating itself, it is less indicative of NMAR behavior.

Therefore,r eview is the strongest NMAR candidate because its absence likely depends on the users' choice to leave a review, which is influenced by their personal experience with the recipe—something that is unobserved when the review is missing.

<iframe
  src="assets/review_missingness.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<iframe
  src="assets/review_rate_kde.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


The empirical distribution of the Total Variation Distance (TVD) for **Review Missingness** shows that the observed TVD (red line) is positioned toward the higher end of the permutation distribution. The **p-value of 0.048** suggests that the observed TVD is relatively rare under the null hypothesis, indicating a **statistically significant relationship** between rating missingness and review missingness at a **5% significance level**. This implies that **review missingness is likely dependent on rating missingness**, meaning that whether a review is missing could be systematically related to whether the rating itself is missing.

<iframe
  src="assets/description_missingness.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<iframe
  src="assets/desc_rate_kde.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The empirical distribution of the Total Variation Distance (TVD) for Description Missingness suggests that the observed TVD (red line) is within the range of TVDs generated by the permutation test. The p-value of 0.548 indicates that the observed TVD is not significantly different from the null distribution, meaning there is little to no strong dependency between rating missingness and description missingness. Since the observed TVD is relatively small compared to the permuted values, we fail to reject the null hypothesis, suggesting that description missingness is likely independent of rating missingness.

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

We plan to predict the average rating of a recipe. This would be a regression type of problem since average ratings are continuous numerical values. While an individual may leave an exact star rating, the average rating of a recipe is often a reflection of several users' preferences. 

We chose average rating because it is the best representation of perception of a rating. It can indicate the quality of a recipe and is what most people assess when trying to find a recipe. From our previous analyses, we found that carbohydrate and protein content of recipes significantly influence their ratings and we may be able to use this information to predict the ratings. 

To evaluate our model, we will use Mean Absolute Error (MAE) since it measures the average magnitude of errors in predictions without considering their direction, making it easy to interpret in the context of recipe ratings. It penalizes large errors linearly, unlike Mean Squared Error (MSE), which squares large errors and can be overly sensitive to outliers.
Since our prediction task involves real-valued ratings, classification metrics like accuracy or F1-score are not relevant. We also report R² (coefficient of determination) as a secondary metric to indicate how well our model explains variance in ratings.

The information available at the time of prediction would be all the columns from the `recipes` and `reviews` datasets, along with any new columns we have made engineered from the existing ones. `ratings` and `avg_ratings` will be removed so that the model only relies on this information. 

## Baseline Model


## Final Model



## Fairness Analysis

