# Analysis of "Carry" Ability by Position in League of Legends

by cozhu (cozhu@ucsd.edu)


## Introduction

**General Introduction**

League of Legends (LOL) is a highly popular multiplayer online battle arena (MOBA) game developed by Riot Games. With a massive global player base, it has become one of the most influential and widely played esports titles in the gaming industry. The dataset used in this analysis is provided by Oracle’s Elixir, capturing detailed match data from professional LOL esports competitions throughout 2022.

This dataset offers a comprehensive array of gameplay statistics and outcomes from a vast collection of LOL matches, providing valuable insights into player behavior, team dynamics, and overall match performance. The dataset includes various features such as individual player performance metrics, positional effectiveness, in-game economics, and broader match dynamics.

In the context of League of Legends, the concept of "carry" refers to the ability of a player to have a significant impact on the outcome of the match, often through superior performance in key areas such as kills, damage dealt, and in-game decision-making. This project specifically focuses on investigating which position—ADC (Bot lane) or Mid lane—is more likely to "carry" the game. We will analyze player performance metrics, in-game statistics, and overall contribution to the team's success to answer this question.

The central question we are interested in is: **Which position, ADC or Mid lane, is more likely to "carry" the game?** We will use data analysis techniques to evaluate the performance of players in these positions, examining key metrics such as kills, deaths, assists, damage dealt, gold earned, and their overall impact on match outcomes. By analyzing these statistics, we aim to identify trends and develop a deeper understanding of positional effectiveness in carrying games.

#### Introduction of Columns

The dataset introduces a range of columns that encapsulate essential gameplay statistics and match outcomes from professional League of Legends esports matches. Below is an introduction to the key columns that will be used in our analysis:

- **gameid**: This column represents a unique identifier for each individual match played. It allows us to distinguish between different matches in the dataset.
  
- **position**: The `position` column specifies the role or position played by an individual player within their team composition. Common positions include `top`, `jungle`, `mid`, `bot`, and `support`. This column is critical to our analysis as we compare the performance of ADC (Bot lane) and Mid lane.

- **result**: This column indicates the outcome of a match for a specific team or player. A value of `1` indicates that the team or the team that the player is in won, while `0` indicates a loss.

- **kills**: The `kills` column quantifies the number of enemy champions a player successfully eliminated during the match. This metric is essential for assessing a player's ability to secure kills and contribute to their team’s victory.

- **deaths**: The `deaths` column records the number of times a player was eliminated by enemy champions. A lower death count typically suggests better survivability and decision-making.

- **assists**: The `assists` column records the number of assists credited to a player, indicating instances where they contributed to eliminating an enemy champion without securing the kill themselves. Assists are important for understanding a player's teamwork and support effectiveness.

- **damagetochampions**: This column captures the total damage dealt by a player to enemy champions. High damage output is often associated with a player’s ability to "carry" the game.

- **totalgold**: The total amount of gold a player has earned during the match. Gold is crucial for purchasing items that enhance a player’s strength and contribute to their ability to carry the game.

- **total cs**: This column records the total number of minions or neutral monsters slain by a player during the match. This metric reflects a player's ability to efficiently farm gold and experience points, which are vital for character progression and itemization.

- **earned gpm**: The `earned gpm` (Gold Per Minute) column shows the rate at which a player earns gold throughout the game, indicating economic efficiency.

- **goldat10, goldat15, goldat20, goldat25**: These columns represent the amount of gold a player has at various points in the game (10, 15, 20, and 25 minutes). They provide insights into a player’s early, mid, and late-game economic performance.

- **xpat10, xpat15, xpat20, xpat25**: These columns represent the amount of experience points (XP) a player has at various points in the game. Experience contributes to leveling up and gaining stronger abilities, which is critical for carrying games.

- **csat10, csat15, csat20, csat25**: These columns represent the number of minions or monsters slain by a player at different times during the match. This metric helps in understanding a player’s farming efficiency over time.

- **golddiffat10, golddiffat15, golddiffat20, golddiffat25**: These columns represent the gold difference between the player and their opponent at different times during the match. Positive values indicate that the player has more gold than their opponent, which can be an indicator of dominance in the game.

- **xpdiffat10, xpdiffat15, xpdiffat20, xpdiffat25**: These columns represent the experience point difference between the player and their opponent. Higher experience can give a player an advantage in levels and abilities.

- **csdiffat10, csdiffat15, csdiffat20, csdiffat25**: These columns represent the difference in minion or monster kills between the player and their opponent. A higher CS (creep score) difference can reflect better farming and lane control.

By focusing on these key columns, we will conduct a thorough analysis to determine which position is more likely to "carry" a game, providing insights that can influence strategic decision-making in professional League of Legends play.

## Cleaning and EDA

**Data Clean**
### Data Cleaning Steps:

During the data cleaning process, several key steps were taken to ensure the dataset was suitable for analysis and model building. These steps involved handling missing values, transforming data, and preparing it for the prediction tasks. The dataset used for analysis consists of player-level performance metrics across various game attributes.

#### Steps Taken:

1. **Removing Irrelevant Columns**:
   - Initially, irrelevant columns that did not contribute to the analysis were dropped. Columns like `url` and `year` were removed as they are identifiers and do not provide meaningful information for predicting player performance.
   
2. **Dropping Team Summary Rows**:
   - The dataset originally included rows representing team-level summaries alongside player-level data. Since our analysis focuses on individual player performance, these team-level rows were dropped to avoid redundant or conflicting information in the analysis.

3. **Retaining Key Performance Metrics**:
   - Key columns such as `kills`, `deaths`, `assists`, `damagetochampions`, `totalgold`, and time-based statistics (`goldat10`, `csat15`, etc.) were retained for analysis, as these variables provide essential information on player performance across different game stages.

#### Final Cleaned Data:

After the above steps, the dataset was saved to a new file (`cleaned_lol_data_players_only.csv`), which contains only the player-specific data, with the relevant features for analysis. Here is a sample of the cleaned dataset:

```python
print(cleaned_lol_data.head())
```

| gameid              | teamname           | position | result | kills | deaths | assists | gamelength | damagetochampions | totalgold | ... | csdiffat25 |
|---------------------|--------------------|----------|--------|-------|--------|---------|------------|-------------------|-----------|-----|------------|
| ESPORTSTMNT01_2690210 | BRION Challengers | top      | 0      | 2     | 3      | 2       | 1713       | 15768.0           | 10934     | ... | 9          |
| ESPORTSTMNT01_2690210 | BRION Challengers | jng      | 0      | 2     | 5      | 6       | 1713       | 11765.0           | 9138      | ... | -28        |
| ESPORTSTMNT01_2690210 | BRION Challengers | mid      | 0      | 2     | 2      | 3       | 1713       | 14258.0           | 9715      | ... | -5         |
| ESPORTSTMNT01_2690210 | BRION Challengers | bot      | 0      | 2     | 4      | 2       | 1713       | 11106.0           | 10605     | ... | -85        |
| ESPORTSTMNT01_2690210 | BRION Challengers | sup      | 0      | 1     | 5      | 6       | 1713       | 3663.0            | 6678      | ... | 12         |

This cleaned dataset contains crucial information for further analysis, including player statistics, game results, and performance metrics at different time points in the game.

**Univariate Analysis**
We permformed univariate analysis on the ['totalgold', 'damagetochampions', 'total cs'] statistics in the dataset

### 1. **Total CS (Creep Score) Distribution**
   - **Chart Type**: Histogram (with KDE curve)
   - **Description**:
     This chart displays the **total creep score (CS)** distribution, which measures the number of minions and neutral monsters a player kills in the game. CS is an essential indicator of a player's economic performance and growth in the game. The X-axis represents the number of creep kills, while the Y-axis shows the number of players with that specific CS.
     
     This chart allows us to observe the concentration of CS among players, where higher CS typically indicates better lane farming and resource acquisition abilities. The KDE curve further helps us understand the density of CS across different ranges, offering insights into how players' growth is distributed in the game.
<iframe src="assets/total cs_distribution.html" width=800 height=600 frameBorder=0></iframe>

### 2. **Total Gold Distribution**
   - **Chart Type**: Histogram (with KDE curve)
   - **Description**:
     This chart shows the distribution of **total gold** earned by players in the game. The X-axis represents the total amount of gold earned by players, while the Y-axis shows the number of players with that amount of gold.
     
     From this chart, we can see that most players' gold is concentrated in a certain range, which could indicate the typical gold amount earned during an average game duration or standard gameplay. The KDE curve shows the density of the distribution, helping to visualize the trends and concentration areas of the gold distribution.

<iframe src="assets/totalgold_distribution.html" width=800 height=600 frameBorder=0></iframe>

### 3. **Damage to Champions Distribution**
   - **Chart Type**: Histogram (with KDE curve)
   - **Description**:
     This chart illustrates the distribution of **damage to champions** dealt by players during the game. The X-axis represents the total amount of damage dealt to enemy champions, and the Y-axis shows the number of players dealing that amount of damage.
     
     By analyzing this chart, we can observe the spread of damage among players, identifying which players deal more damage and which deal less. The KDE curve provides additional insights into the density of damage distribution, highlighting concentrated areas where the most damage is dealt.

<iframe src="assets/damagetochampions_distribution.html" width=800 height=600 frameBorder=0></iframe>

**Bivariate Analysis**: In the bivariate analysis, I explored the relationships between the following pairs:  
- ('position', 'kills')  
<iframe src="assets/position_vs_kills.html" width=800 height=600 frameBorder=0></iframe>
- ('position', 'totalgold')  
<iframe src="assets/position_vs_totalgold.html" width=800 height=600 frameBorder=0></iframe>
- ('position', 'damagetochampions')  
<iframe src="assets/position_vs_damagetochampions.html" width=800 height=600 frameBorder=0></iframe>

**Aggregated Analysis**

- Average deaths, total gold, and total CS grouped by position: Compare the average values of these key statistics for players in different positions.
- Damage contribution ratio by position: Calculate the average damage contribution for each position and analyze which position dominates in overall damage output.

Here is the provided data converted into **Markdown** format:

---

### Average Stats by Position:

| Position | Kills   | Deaths  | Assists  | Total Gold    | Total CS     | Damage to Champions  |
|----------|---------|---------|----------|---------------|--------------|----------------------|
| bot      | 4.2597  | 2.5471  | 5.3719   | 13654.14      | 278.59       | 18065.73             |
| jng      | 3.0570  | 3.1135  | 6.8492   | 10793.85      | 173.15       | 10224.92             |
| mid      | 3.4999  | 2.6562  | 5.8950   | 12597.68      | 260.90       | 17470.36             |
| sup      | 0.8742  | 3.2438  | 9.1836   | 7600.77       | 35.44        | 5401.55              |
| top      | 2.7935  | 2.9527  | 5.0241   | 12300.37      | 247.52       | 15534.97             |

---

### Damage Share by Position:

| Position | Damage Share |
|----------|--------------|
| bot      | 0.2709       |
| jng      | 0.1533       |
| mid      | 0.2619       |
| sup      | 0.0810       |
| top      | 0.2329       |

**Conclusion**

Through these univariate, bivariate, and interesting aggregated analyses, we can gain a more comprehensive understanding of the performance of different positions in the game, further addressing the core question of which position is more likely to "carry" the game.

## Assessment of Missingness

### NMAR Analysis

In our data, we believe the columns `kills`, `deaths`, `assists`, `damagetochampions`, `totalgold`, `total cs`, `earned gpm`, `goldat10`, `goldat15`, `goldat20`, `goldat25`, `xpat10`, `xpat15`, `xpat20`, `xpat25`, `csat10`, `csat15`, `csat20`, `csat25`, `golddiffat10`, `golddiffat15`, `golddiffat20`, `golddiffat25`, `xpdiffat10`, `xpdiffat15`, `xpdiffat20`, `xpdiffat25`, `csdiffat10`, `csdiffat15`, `csdiffat20`, and `csdiffat25` are all Not Missing At Random (NMAR).

By examining these columns, we see that their missingness does not exhibit any clear trends depending on other columns. In the context of League of Legends matches, the missing data in these columns is typically related directly to the specific course of the match or player behavior. For example, a player may leave the game early, or the match may end prematurely, resulting in these statistics not being recorded properly. In this case, we believe these columns are NMAR because their missingness is determined by specific events during the match, rather than by other observable variables.

To make these columns Missing at Random (MAR), we might need to introduce additional variables to explain the missingness. For example, variables that record whether a player left the match early or whether the match ended prematurely could help explain why these values are missing.
**Missingness Dependency**

In this analysis, we will explore whether the missingness of the `goldat20` column depends on other key variables, specifically `damagetochampions` and `total cs`. The `goldat20` metric represents the amount of gold a player has accumulated at the 20-minute mark in a game, which is crucial for analyzing player performance and team economy. However, we observed that `goldat20` has some missing values in the dataset. To better understand the reasons behind these missing values, we need to assess whether the missingness of `goldat20` is related to other key performance indicators in the game.
We selected two variables that might influence the missingness of `goldat20`:
1. damagetochampions: This represents the total damage a player deals to enemy champions. We hypothesize that if a player has dealt relatively low damage during a match, it might indicate poor performance, which could be associated with missing `goldat20` values.

2. total cs: This represents the total number of minions and neutral monsters a player has killed. The CS (Creep Score) is a vital indicator of a player’s economy and development speed. We aim to investigate whether the missingness of `goldat20` is related to the player’s CS performance.

By analyzing these variables, we aim to reveal whether the missingness of `goldat20` is influenced by these key metrics. Understanding these patterns of missingness can not only help us better interpret the data but also guide the data processing and model-building steps that follow.

### Hypothesis Testing
To determine whether the missingness of the goldat20 column depends on other variables, we establish the following hypotheses for our permutation tests:

For damagetochampions:

Null Hypothesis (H₀): The distribution of damagetochampions is the same whether goldat20 is missing or not. In other words, goldat20 missingness is independent of the damagetochampions values.
Alternative Hypothesis (H₁): The distribution of damagetochampions is different when goldat20 is missing compared to when it is not. This would suggest that the missingness of goldat20 is dependent on the damagetochampions values.

For total cs:

Null Hypothesis (H₀): The distribution of total cs is the same whether goldat20 is missing or not. This means that the missingness of goldat20 is independent of the total cs values.
Alternative Hypothesis (H₁): The distribution of total cs is different when goldat20 is missing compared to when it is not. This would indicate that the missingness of goldat20 depends on the total cs values.

Below is the observed distribution of damagetochampions when goldat20 is missing and not missing.
| Damage to Champions Bin | goldat20 = Not Missing | goldat20 = Missing |
|-------------------------|------------------------|--------------------|
| 0                       | 0.400132               | 0.434787           |
| 10000                   | 0.414456               | 0.400377           |
| 20000                   | 0.141752               | 0.126279           |
| 30000                   | 0.033211               | 0.029725           |
| 40000                   | 0.007748               | 0.006839           |
| 50000                   | 0.002050               | 0.001185           |

After we performed permutation tests, we found that the observed statistic for this permutation test is:  685.5564124524335, and the p-value is 0.0. The plot above shows the empirical distribution of the TVD for the test.
<iframe src="assets/damagetochampions_tvd.html" width=800 height=600 frameBorder=0></iframe>

Since the p-value is less than the 0.05 significance level, we reject the null hypothesis. Thus, the missingness of goldat20 depends on the damagetochampions column.

The second permutation test that we are performing is on goldat20 and total cs, and the missingness of goldat20 does not depend on total cs.
Null Hypothesis: The distribution of total cs when goldat20 is missing is the same as the distribution of total cs when goldat20 is not missing.
Alternative Hypothesis: The distribution of total cs when goldat20 is missing is NOT the same as the distribution of total cs when goldat20 is not missing.
Below is the observed distribution of total cs when goldat20 is missing and not missing.

| Total CS Bin | goldat20 = Not Missing | goldat20 = Missing |
|--------------|------------------------|--------------------|
| 0-100        | 0.202173               | 0.205062           |
| 100-200      | 0.236924               | 0.242973           |
| 200-300      | 0.405981               | 0.396015           |
| 300-400      | 0.138681               | 0.137803           |
| 400-500      | 0.015287               | 0.017178           |
| 500-600      | 0.000935               | 0.000969           |

After we performed permutation tests, we found that the observed statistic for this permutation test is: 1.065406542261485, and the p-value is 0.212. The plot above shows the empirical distribution of the TVD for the test.

<iframe src="assets/tvd_chart.html" width=800 height=600 frameBorder=0></iframe>

Since the p-value is greater than the 0.05 significance level, we fail to reject the null hypothesis. Thus, the missingness of goldat20 does not depend on the total cs column.

## Hypothesis Testing

### Hypothesis Testing

In this hypothesis test, we aim to assess whether there is a significant difference in performance between ADC and Mid Lane (Mid) positions in League of Legends games. Specifically, we want to determine which position more often acts as the "carry" role, contributing most to the team's victory.

**Null Hypothesis (H₀)**: There is no significant difference in performance between ADC and Mid, meaning both positions are equally likely to "carry" the game.

**Alternative Hypothesis (H₁)**: There is a significant difference in performance between ADC and Mid, meaning one position is more likely to "carry" the game.

**Test Statistics**: Absolute mean difference in key metrics such as Kills, Damage to Champions, and Total Gold between ADC and Mid.

**Significance Level**: 5%

Here is a histogram containing the distribution of our test statistics during the hypothesis test:
<iframe src="assets/lol_data_comparison.html" width=800 height=600 frameBorder=0></iframe>

Results
Kills:

t-statistic: 28.23
p-value: 5.856e-174
The extremely low p-value indicates a highly significant difference in the number of kills between ADC and Mid. This suggests that one of these positions is likely to secure more kills, potentially contributing more to the team’s success.

Damage to Champions:

t-statistic: 7.76
p-value: 8.561e-15
The low p-value here also indicates a significant difference in the damage dealt to champions between ADC and Mid. This suggests that the two positions differ substantially in their ability to deal damage, which is a critical factor in their ability to carry the game.

Total Gold:

t-statistic: 37.86
p-value: 4.679e-309
The p-value is virtually zero, indicating an extremely significant difference in the total gold earned between ADC and Mid. Since gold is essential for purchasing items that increase a champion's power, this metric strongly suggests that one position is more effective at accumulating resources and thus has a greater carry potential.

Conclusion
Based on the hypothesis tests, we reject the null hypothesis for all three metrics. The results show that ADC and Mid differ significantly in terms of kills, damage to champions, and total gold. Specifically, one of these roles (which can be further analyzed) consistently outperforms the other in these key areas, indicating a higher likelihood of acting as the "carry" in a game.

According to the histograms reveal that the ADC consistently outperforms the Mid in key metrics such as kills, damage to champions, and total gold, then we can confirm the following conclusions:

### Confirmed Conclusions

1. **The ADC outperforms the Mid in terms of kills**:
   - The histograms show that the distribution of kills for the ADC is higher than that of the Mid, indicating that the ADC is more likely to secure more kills in the game. This confirms that the ADC tends to eliminate enemy champions more frequently, which is a key characteristic of a carry role.

2. **The ADC performs better in terms of damage to champions**:
   - The histograms show that the ADC surpasses the Mid in dealing damage to champions, confirming that the ADC typically takes on a greater role in damage output during the game, making them more influential in battles.

3. **The ADC excels in resource accumulation (total gold)**:
   - The histograms show that the ADC has a higher total gold accumulation compared to the Mid, confirming that the ADC has an advantage in gathering resources and purchasing items, further enhancing their carry potential in the game.

### Summary

The confirmed conclusion from the histograms is that, in these games, the ADC is more likely to serve as the carry role compared to the Mid. The ADC’s higher kill count, greater damage output, and superior gold accumulation give them a larger impact on the game and may be the key to the team’s victory.

This conclusion can serve as a foundation for further analysis and strategic adjustments, helping to optimize team performance in matches. 

## Framing a Prediction Problem
Problem Identification:

In the previous steps, we analyzed the performance differences across different positions in League of Legends. Based on these analyses, we will build a predictive model to explore how we can predict a player’s specific position based on their in-game statistics.

Prediction Problem:

Our prediction problem is: “Based on a player’s in-game statistics, can we predict their position (Top, Jungle, Mid, ADC, or Support)?”

Type: Classification problem (Multiclass Classification)
Response Variable: position (the player’s position). We aim to predict whether the player’s position is Top, Jungle, Mid, ADC, or Support.
Feature Selection:

For building the model, we will use the following features:

kills: The number of enemy champions the player has killed during the game.
deaths: The number of times the player has been killed by enemy champions.
assists: The number of assists the player has provided when their teammates killed enemy champions.
damagetochampions: The total amount of damage the player has dealt to enemy champions.
totalgold: The total amount of gold the player has accumulated during the game.
total cs: The total number of minions and monsters the player has killed.
goldat10, goldat15, goldat20, goldat25: The amount of gold the player has at 10, 15, 20, and 25 minutes into the game.
xpat10, xpat15, xpat20, xpat25: The player’s experience (XP) at 10, 15, 20, and 25 minutes into the game.
csat10, csat15, csat20, csat25: The player’s creep score (CS) at 10, 15, 20, and 25 minutes into the game.
These features directly reflect the player’s performance in the game and their ability to accumulate resources and output damage at different stages of the match.

Data Preprocessing:

We will one-hot encode the position column, generating five binary columns representing Top, Jungle, Mid, ADC, and Support. This way, our model will be able to predict which of these five positions the player belongs to.

Splitting Training and Test Data:

To prevent overfitting, we will split the data into training and test sets. 75% of the data will be used to train the model, and 25% will be used to test the model.

Evaluation Metrics:

In a multiclass problem, we can use accuracy as the primary evaluation metric. Additionally, we can consider using macro F1-score, as it better evaluates the overall performance of the model in the presence of class imbalance. The macro F1-score averages the F1-scores of all classes, making it suitable for multiclass tasks with imbalanced data.

Feature Selection Timing:

At the time of prediction, we will only use the statistics that are available during the game, such as kills, deaths, assists, damage to champions, total gold, and the experience and creep score available at early stages of the game (e.g., 10, 15 minutes). This ensures the model’s validity and avoids data leakage.

## Baseline Model
**Model Overview**:

In the previous steps, we defined a multi-class classification problem to predict a player’s position in League of Legends (Top, Jungle, Mid, ADC, Support). Now, we will build a baseline model using logistic regression to predict the player’s position.

Feature Selection:

We selected the following features for training the model:

kills: The number of enemy champions the player has killed during the game.

deaths: The number of times the player has been killed by enemy champions.

assists: The number of assists the player has provided when their teammates killed enemy champions.

damagetochampions: The total amount of damage the player has dealt to enemy champions.

totalgold: The total amount of gold the player has accumulated during the game.

total cs: The total number of minions and monsters the player has killed.

goldat10, goldat15, goldat20, goldat25: The amount of gold the player has at 10, 15, 20, and 25 minutes into the game.

xpat10, xpat15, xpat20, xpat25: The player’s experience (XP) at 10, 15, 20, and 25 minutes into the game.
csat10, csat15, csat20, csat25: The player’s creep score (CS) at 10, 15, 20, and 25 minutes into the game.

All of these features are quantitative, so no additional encoding or transformation is necessary. However, to ensure the model trains and predicts effectively, we will standardize these features.

Data Preprocessing and Model Training:

We will use a Pipeline in scikit-learn to handle the entire data processing and model training process.

**Feature Types**:

All features in this model are quantitative, so no encoding was necessary.
We standardized the features to ensure the model can effectively compare between features with different scales.
Model Performance:

The logistic regression model provides a simple baseline to initially evaluate the relationship between these features and the player’s position.
While accuracy is a crucial metric, we also need to examine other metrics in the classification report (such as precision, recall, and F1-score) to fully assess the model's performance.

**Model Evaluation**
Classification Report:

Bot:
Precision: 0.88
Recall: 0.87
F1-Score: 0.88
Support: 6117 instances
Jungle (Jng):
Precision: 0.90
Recall: 0.95
F1-Score: 0.92
Support: 6241 instances
Mid:
Precision: 0.71
Recall: 0.70
F1-Score: 0.71
Support: 6283 instances
Support (Sup):
Precision: 0.97
Recall: 0.97
F1-Score: 0.97
Support: 6168 instances
Top:
Precision: 0.72
Recall: 0.70
F1-Score: 0.71
Support: 6283 instances
Overall Metrics:

Accuracy: 0.837
Macro Average:
Precision: 0.84
Recall: 0.84
F1-Score: 0.84
Weighted Average:
Precision: 0.84
Recall: 0.84
F1-Score: 0.84
Interpretation:

The model performs quite well overall, with an accuracy of approximately 83.7%.
The Support and Jungle roles show the highest performance, with F1-scores of 0.97 and 0.92, respectively. These roles are predicted with high precision and recall.
The Mid and Top roles have slightly lower performance, with F1-scores around 0.71. This suggests that these roles are more challenging for the model to predict, potentially due to overlap in features with other roles.
The balanced precision, recall, and F1-scores indicate that the model is not overly biased towards any particular role, although some roles are easier to predict than others.
Conclusion
The logistic regression model provides a solid baseline with a good overall accuracy of 83.7%. However, there is room for improvement, particularly for the Mid and Top roles.Logistic regression as a baseline model has certain limitations, especially in cases where the data relationships are complex and nonlinear. In subsequent steps, we can explore using more sophisticated models (e.g., Random Forest, Gradient Boosting, or Neural Networks) to improve performance.

## Final Model

**Model Improvement Strategy**:

To improve our baseline model, we will execute the following steps:

Feature Engineering: Extract new features from the existing data and transform current features to enhance the model’s predictive performance.

New Feature 1: Kill/Death/Assist ratio (KDA). This feature better reflects the player’s performance and its impact on the game outcome.
New Feature 2: Gold per Minute (GPM). By dividing the total gold by game duration, we can obtain a better measure of the player’s economic capability.
Hyperparameter Tuning: Use GridSearchCV to tune the hyperparameters of the Logistic Regression model and find the optimal configuration.

Model Selection: We will continue to use the Logistic Regression model as the final model, but we can also experiment with other models (e.g., Random Forest) and select the best-performing one.

**First Try Result**

'First Try' Accuracy: 0.8435
Best Parameters: {'classifier__C': 1, 'classifier__penalty': 'l2'}

Summary:

Feature Engineering:

We introduced two new features: KDA (Kills/Deaths/Assists Ratio) and GPM (Gold Per Minute).
KDA: This feature provides a comprehensive measure of a player's performance by combining kills, assists, and deaths, offering a more nuanced indicator of a player's impact on the game.
GPM: This feature represents the player's economic efficiency during the game, which is particularly important for roles like ADC and Jungle, where gold accumulation is crucial.
Hyperparameter Tuning:

Using GridSearchCV, we optimized the regularization parameter (C) for the logistic regression model. The best-performing parameter was C = 1 with an L2 penalty.
This tuning helped refine the model, preventing overfitting and ensuring better generalization to unseen data.
Model Performance:

The 'First Try' achieved an accuracy of 84.35%, which is an improvement over the baseline model's accuracy of 83.66%.
This indicates that the additional features and optimized hyperparameters contributed to a better understanding of the player positions, resulting in improved predictive performance.
Comparison with Baseline Model:

Baseline Model Accuracy: 83.66%
'First Try' Accuracy: 84.35%
The increase in accuracy, though modest, reflects the effectiveness of feature engineering and hyperparameter tuning in improving the model's performance.
Conclusion:

The final model demonstrates a clear improvement over the baseline model. The introduction of KDA and GPM as new features allowed the model to capture more complex relationships in the data, leading to better predictions of player positions in League of Legends.
The optimized hyperparameters further enhanced the model's performance, making it more robust and accurate.

**"Although there has been a slight improvement, it is still not good enough, so further optimization is possible."**

"Improvement Strategy: Using Gradient Boosting Machines (GBM) and Feature Interaction

**Overview**: We will improve the model's performance by introducing a more complex model—Gradient Boosting Machines (GBM)—and incorporating feature interaction. GBM often excels at handling data with complex non-linear relationships. Additionally, feature interaction can help the model capture intricate relationships between multiple features.

**Implementation Steps**:

1. **Introducing Feature Interaction**:
   - We will create new features by combining existing ones through interaction. For example, multiplying *kills* and *totalgold* can capture the relationship between kill count and economic performance.
   - These interaction features will be added to the dataset as input for the model.

2. **Using Gradient Boosting Machines**:
   - We will use XGBoost as the implementation of GBM and optimize the model's performance through hyperparameter tuning.
   - GridSearchCV will be used to fine-tune key XGBoost hyperparameters such as learning rate, max depth, and subsample ratio."

   **Final Model Accuracy**: 87.04%  
**Best Parameters**: {'subsample': 0.8, 'n_estimators': 300, 'max_depth': 7, 'learning_rate': 0.1, 'colsample_bytree': 1.0}

#### Performance Summary:

The final model achieved an overall accuracy of **87.04%** across all roles. Below is a breakdown of the model's performance for each class (role):

| Class | Precision | Recall | F1-Score | Support |
|-------|-----------|--------|----------|---------|
| 0 (Bot)  | 0.93      | 0.93   | 0.93     | 6117    |
| 1 (Jungle) | 0.96      | 0.98   | 0.97     | 6241    |
| 2 (Mid)   | 0.74      | 0.73   | 0.74     | 6283    |
| 3 (Support) | 0.98      | 0.98   | 0.98     | 6168    |
| 4 (Top)   | 0.74      | 0.74   | 0.74     | 6283    |

- **Precision**: Measures how many of the predicted positives are actually correct. The precision is highest for the Support role at **98%**, followed closely by Jungle (**96%**) and Bot (**93%**).
  
- **Recall**: Indicates the ability of the model to find all the positive instances. The recall for Jungle is the highest at **98%**, which indicates that the model can effectively identify players in this role. Mid and Top roles have lower recall scores (**73-74%**), showing there is room for improvement in these roles.

- **F1-Score**: A balance between precision and recall. Jungle and Support roles have an F1-score of **97-98%**, indicating strong model performance. Mid and Top have an F1-score of **74%**, indicating a need for further model refinement for these positions.

#### Model Improvement:

1. **Best Parameters**:
   - The best model parameters were obtained through hyperparameter tuning, with the following configuration:
     - **Subsample**: 0.8
     - **n_estimators**: 300
     - **Max Depth**: 7
     - **Learning Rate**: 0.1
     - **Colsample_bytree**: 1.0

   These parameters balance the model's complexity, depth, and number of estimators, allowing it to capture non-linear patterns in the data without overfitting.

2. **Areas of Strength**:
   - The model performs exceptionally well in predicting Jungle and Support roles, as reflected by their high precision, recall, and F1-scores.
   - These two roles have the strongest representation in the dataset, and the model is effective at distinguishing them from other positions.

3. **Areas for Improvement**:
   - The Mid and Top roles have lower F1-scores, reflecting challenges in accurately classifying these positions. Further feature engineering or advanced modeling techniques could improve classification for these roles.
   - Additional features, such as more detailed game-specific statistics or interactions between features, may help enhance model performance for Mid and Top roles.

#### Conclusion:

The final model demonstrates strong overall performance, particularly in classifying Jungle and Support roles, where the precision, recall, and F1-scores are exceptionally high. While the accuracy for Mid and Top roles is acceptable, there is potential for further refinement through feature engineering or alternative modeling techniques. Overall, the model provides a solid foundation for predicting player roles in League of Legends based on in-game statistics.

## Step 8: Fairness Analysis
### In our model, we decided to compare the Bot and Mid positions to detect whether there are significant differences in the model's performance for these two roles. We will use **precision** as the evaluation metric to analyze the difference in precision between these two roles.

#### Group Selection:
- **Group X (Bot)**: Players with the role of Bot.
- **Group Y (Mid)**: Players with the role of Mid.

#### Evaluation Metric:
We choose **precision** as the evaluation metric because it reflects how accurate the model is when predicting a certain role as positive. Since the functions and performances of different roles in the game may vary, precision is an effective metric for assessing the model’s performance differences across roles.

#### Hypotheses:
- **Null Hypothesis**: The model is fair, and the precision for Bot and Mid roles is roughly the same, with any difference due to random causes.
- **Alternative Hypothesis**: The model is unfair, and the precision for the Bot role is significantly higher than for the Mid role.

#### Test Statistic:
We select the **absolute difference in precision** as the test statistic, which calculates the difference in precision between the Bot and Mid groups.

#### Permutation Test Steps:
1. Calculate the precision of the model for both Bot and Mid groups.
2. Randomly shuffle the Bot and Mid labels and recalculate the precision difference between these groups. Repeat this process multiple times to generate a distribution of the test statistic.
3. Calculate where the actual precision difference lies in relation to the permutation distribution, and derive the p-value.
**Result Interpretation:**

**Observed difference = 0.0:**

This means that the accuracy of the model (or any other statistical measure you used) is exactly the same between the two groups being compared. In other words, the model's performance shows no difference between these two groups.

**p-value = 1.0:**

The p-value is calculated in the permutation test and is used to measure whether the observed difference is statistically significant. A smaller p-value indicates that the observed difference is less likely to be due to randomness. A p-value of 1.0 indicates that the observed difference between the two groups can be fully explained by randomness, and therefore, there is no reason to believe that the model is unfair in its performance between these two groups.

**Conclusion:**

Based on these results, we can draw the following conclusions:

- **Fairness Analysis Result**: The model performs fairly between the two selected groups (bot and mid), with no significant difference detected.
- **No Significant Difference**: Since the Observed difference is 0 and the p-value is 1.0, this indicates that the model's accuracy in predicting the bot and mid roles is the same, and any minor differences can be entirely attributed to random fluctuations.