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

<iframe src="assets/10-80-enrollment.html" width=800 height=600 frameBorder=0></iframe>

---

## Assessment of Missingness

Here's what a Markdown table looks like. Note that the code for this table was generated _automatically_ from a DataFrame, using

```py
print(counts[['Quarter', 'Count']].head().to_markdown(index=False))
```

| Quarter     |   Count |
|:------------|--------:|
| Fall 2020   |       3 |
| Winter 2021 |       2 |
| Spring 2021 |       6 |
| Summer 2021 |       4 |
| Fall 2021   |      55 |

---

## Hypothesis Testing


---