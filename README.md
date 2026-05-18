# NBA Salary and Performance Analysis 2025-2026

This project uses exploratory data analysis (EDA) and multiple linear regression to analyze the relationship between NBA player performance metrics and player salaries. The project also develops multiple linear regression models to estimate player salaries using performance statistics.

---

## Table of Contents
- [Project Overview](#project-overview)
- [Key Insights](#key-insights)
- [Dataset](#dataset)
- [Objectives](#objectives)
- [Technologies Used](#technologies-used)
- [Exploratory Data Analysis](#exploratory-data-analysis)
- [Modeling](#modeling)
- [Results](#results)
- [How to Run](#how-to-run)
- [Author](#author)

---

## Project Overview

The goal of this project is to explore relationships between NBA player performance stats and annual salaries using exploratory data analysis and regression modeling techniques. This analysis can be used to evaluate whether player salaries align with expected on-court performance, estimate potential contract value, and identify potentially undervalued or overvalued player contributions. To achieve this goal, I first inspected the distributions of player stats such as points per game; then, I examined relationships between different performance metrics and a player's salary. I then created and evaluated two multiple linear regression models: one using manually selected features and another using sequential feature selection.

---

## Key Insights

 - Player performance stats are not valued equally as some stats have a much stronger correlation with a player's salary than others, but almost all performance metrics have at least some correlation with salary.
 - Points per game is overwhelmingly the player stat that is most associated with a player's salary.
 - Though it is not a perfect representation of the relationship between NBA player performance metrics and salary, a linear regression model works reasonably well to predict salary based on in-game stats.
 - Over half of the variation in player salaries can be explained by a linear regression model.

---

## Dataset

The dataset is a combination of a salary dataset and a player performance dataset. I cleaned the data by removing players with missing salary values and reformatting names to match between datasets (i.e. Dončić vs Doncic) then performed an inner merge on the datasets. This merge removed players who were missing from the salary dataset because they are on two-way contracts that split their time between the NBA and NBA G-League, as well as players who missed the entirety of the 2025-26 NBA season due to injury or suspension. Finally, I removed players that did not play a significant amount of games or minutes and changed the players' stats from a "total" format to a "per game" format (i.e. from total rebounds to rebounds per game).

### Player Stat Dataset:

- [NBA Player Stats Dataset 2026](https://www.kaggle.com/datasets/nilesh2042/nba-player-stats-2026)
- Rows/Columns: 530/26
- Includes player names and teams as well as performance statistics such as games played, minutes played, points scored, rebounds, assists, steals, blocks, shooting, percentages, turnovers, efficiency rating, and more.

### Salary Dataset:

- [2025-26 NBA Player Contracts](https://www.basketball-reference.com/contracts/players.html)
- Rows/Columns: 490/5
- Includes player names, teams, salary in current and future seasons, and total guaranteed earnings.


Combined Dataset Shape: 455 rows, 30 columns

---

## Objectives

- Perform exploratory data analysis (EDA)
- Identify key player stats affecting a player's salary
- Clean and reformat data
- Train and evaluate regression models
- Evaluate model performance

---

## Technologies Used

 - Python
 - Jupyter Lab
 - Pandas
 - Scikit-Learn
 - Matplotlib
 - Seaborn
 - Statsmodels

---

## Exploratory Data Analysis

- Computed the player salary quartiles and made a density plot of salaries to get a general understanding of the salary distribution.
- Distribution is skewed right with the majority of salaries falling below $10 million and a relatively small portion of salaries above $20 million.

- Made a set of density plots for the offensive and defensive performance stats to understand the distributions of player metrics.
  - Distributions for each stat vary in spread but are all generally unimodal.
  - Most of the offensive distributions are skewed right like the salary distribution, but free throw percentage and three point percentage are slightly skewed left.
  - For defensive stats, rebounds per game and steals per game have relatively wide distributions while steal-turnover ratio and blocks per game have much tighter distributions.
 
- Plotted interactions between player salaries and some potentially important player stats.
- Based on the scatter plots, points per game has the strongest correlation with player salary.
- Rebounds per game and steals per game look to have a weak correlation with salary.
- 3pt percentage, assist-turnover ratio, and blocks per game do not look to have a significant connection to player salary.

- Made correlation heatmap with seaborn to visualize correlations between variables.
- The metrics that proved to be most correlated with a player's salary were minutes per game, assists per game, rebounds per game, steals per game, and especially points per game.
- Blocks per game are seemingly valued less than these other stats.
- Strong correlation between minutes per game and other game-level stats such as points per game.

### EDA Images

Salary Density Plot:

![NBA Salary Density Plot](Images/nba_salary_density.png)

Scatterplots with relationship between salary and performance statistics:

![Relationship Scatterplots](Images/nba_scatters.png)

Correlation Heatmap:

![Correlation Heatmap](Images/nba_heatmap.png)


---

## Modeling

I used the player performance metrics to create a multiple linear regression model with scikit-learn for a player's salary. After experimenting to find some unhelpful variables, I removed stats which were not formatted as ratios, which gave usable results. This model had an adjusted R² value of around 0.521, and therefore explained more than half of the variation in player salaries.

Data was split into training (75%) and testing (25%) datasets to evaluate model performance on unseen data.

### Feature Selection

- I experimented with different configurations of a sequential feature selector for a second model
- Used backward sequential feature selecting to create a model with 3 features that achieved slightly better performance than the more general model.
- The data for this new model did not include "Guaranteed" or "MIN".
  - Guaranteed was too correlated with salary as an aspect of the player's contract.
  - MIN counted total minutes over the courses of the season and yielded weaker models when the feature selector chose it over MPG.
- Achieved adjusted R² of around 0.538
- Model uses 3-point percentage, points per game, minutes per game
- Somewhat small pool of players and limited number of features means that the model is vulnerable to overfitting.

### Regression Diagnostics

- QQ plot showed approximately normal residuals
- Residual vs. fitted plots suggested minor non-linearity
- Scale-location plot indicated slight heteroscedasticity
- Influence plots revealed several notable outliers
- Independence assumption may be partially violated due to contracts and performances being linked to the player's team

---

## Results

Intuitively, it makes sense that player performances and player salaries are linked. However, by exploring player data from the 2025-26 NBA season and using regression modeling, I have highlighted the player metrics that have the greatest connection with the players' salaries and have constructed a model to estimate what a player's salary could be based upon their performances.

| Model | Features | Adjusted R² | RMSE |
|------|------|------|------|
| Manual Feature Selection | Multiple performance metrics | 0.521 | $8,836,562 |
| Sequential Feature Selection | PPG, MPG, 3P% | 0.538 | $8,656,393 |

Both models performed similarly, however the manually-selected model performs more consistently when the random seed of the test-train split changes.

### Example Predictions

| Player | Predicted Salary  | Actual Salary | Residual |
|------|------|------|------|
| Giannis Antetokounmpo | 50,470,855 | 54,126,450 | 3,655,595 |
| Anthony Edwards | 40,092,984 | 45,550,512 | 5,457,528 |
| Keldon Johnson | 15,465,887 | 17,500,000 | 2,034,113 |
| Josh Okogie | 1,539,256 | 2,296,274 | 757,018 |
| Nikola Vučević | 22,122,636|  21,481,481  | -641,155 |

### Drawbacks of Analysis

 - With a somewhat small total 455 of eligible players for analysis, it was difficult to remove many features from the model without overfitting. The sequential feature selector, for example, chose different features when different random seeds were used. However, there are methods to remedy this such as k-fold cross validation.
 - Players' performances are linked with their team's performances which could greatly alter the relationship between their performance and salary.
 - While the minutes played stat was used in modeling, it could be argued that this is not necessarily a measure of a player's impact on the court and instead is a stat largely controlled by a player's coach. However, it still likely generally reflects a player's importance to their team as more impactful players will likely be given more minutes. This could also help to account for less "measurable" contributions that a player makes to their team.
 - A handful (<20) of players lacked salary data in the contract database.
 - The salary of players on their rookie contracts are based solely on their draft position and not their NBA performances.

### Takeaways

 - The salary distribution was skewed to the right, reflecting that the majority of players make a similar amount, less than $10 million per year, while some highly-paid players make almost $60 million per year.
 - Points per game is linked much more heavily to a player's salary than any other stat.
 - Games Played, Assist-Turnover Ratio, and 3pt FG Percentage had little correlation with player salary.
 - A small majority of the variation in a player's salary can be explained by using a linear regression model with a player's performance statistics.

---

## Skills Demonstrated

- Exploratory Data Analysis (EDA)
- Data Cleaning and Preprocessing
- Data Visualization
- Multiple Linear Regression
- Feature Selection Algorithm
- Regression Diagnostics
- Statistical Analysis
- Predictive Modeling

---

## Repository Structure

```bash
NBA-Salary-and-Performance-Analysis-2025-2026/
│
├── Data/
├── Images/
├── Notebooks/
├── README.md
└── requirements.txt
```

## How to Run

Clone the repository:

```bash
git clone https://github.com/a-g-mikulsky/NBA-Salary-and-Performance-Analysis-2025-2026.git
```

Install dependencies:

```bash
pip install -r requirements.txt
```

Run the notebook:

```bash
jupyter notebook
```

---

## Future Improvements

- Transform player salaries to a log-scale
- Implement k-fold cross validation to improve model robustness
- Incorporate advanced metrics such as PER
- Incorporate more recent NBA seasons
- Create dashboard-style visualization

## Author

Aiden Mikulsky | Data science student at the University of Wisconsin-Madison

- LinkedIn: www.linkedin.com/in/aiden-mikulsky-ab226a337
- Email: aidenmiku920@icloud.com
