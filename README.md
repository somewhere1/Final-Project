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

## Hypothesis Testing
To determine whether the missingness of the goldat20 column depends on other variables, we establish the following hypotheses for our permutation tests:

For damagetochampions:

Null Hypothesis (H₀): The distribution of damagetochampions is the same whether goldat20 is missing or not. In other words, goldat20 missingness is independent of the damagetochampions values.
Alternative Hypothesis (H₁): The distribution of damagetochampions is different when goldat20 is missing compared to when it is not. This would suggest that the missingness of goldat20 is dependent on the damagetochampions values.

For total cs:

Null Hypothesis (H₀): The distribution of total cs is the same whether goldat20 is missing or not. This means that the missingness of goldat20 is independent of the total cs values.
Alternative Hypothesis (H₁): The distribution of total cs is different when goldat20 is missing compared to when it is not. This would indicate that the missingness of goldat20 depends on the total cs values.

Below is the observed distribution of damagetochampions when goldat20 is missing and not missing.
Damage to Champions Bin  goldat20 = Not Missing  goldat20 = Missing