# NBA Salary and Performance Analysis 2025-2026

This project employs exploratory data analysis to examine the relationship between player performance metrics and player salaries in the National Basketball Association (NBA). The project also develops multiple linear regression models to estimate player salaries using performance statistics.

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

The goal of this project is to explore relationships between NBA player performance stats and annual salaries using exploratory data analysis and regression modeling techniques. This information could be used to evaluate whether a player's salary is in line with the expected value for a player with their level of performance, gain an understanding of the potential value of a new contract for a player, or identify which kinds of contributions could be undervalued or overvalued by NBA teams. To achieve this goal, I first inspected the distributions of player stats such as poins per game; then, I examined relationships between different performance metrics and a player's salary. I then created and evaluated two linear regression models for a player's salary with one created manually and the other one using sequential feature selection.

---

## Key Insights



---

## Dataset

The dataset is a combination of a salary dataset and a player performance dataset. I cleaned the data by removing players with missing salary values and reformatting names to match between datasets (i.e. Dončić vs Doncic) then performed an inner merge on the datasets. This merge removed players who were missing from the salary dataset because they are on two-way contracts that split their time between the NBA and NBA G-League, as well as players who missed the entirety of the 2025-26 NBA season due to injury or suspension. Finally, I removed players that did not play a significant amount of games or minutes and changed the players' stats from a "total" format to a "per game" format (i.e. from total rebounds to rebounds per game).

### Player Stat Dataset:

- [NBA Player Stats Dataset 2026](https://www.kaggle.com/datasets/nilesh2042/nba-player-stats-2026)
- Rows/Columns: 530/26
- Includes player names and teams as well as performance statistics such as games played, minutes played, points scored, rebounds, assists, steals, blocks, shooting, percentages, turnovers, efficiency rating, and more.

---

### Salary Dataset:

- [2025-26 NBA Player Contracts](https://www.basketball-reference.com/contracts/players.html)
- Rows/Columns: 490/5
- Includes player names, teams, salary in current and future seasons, and total guaranteed earnings.

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

I computed the player salary quartiles and made a density plot of salaries to get a general understanding of the salary distribution. This distribution is skewed right with the majority of salaries falling below $10 million and a relatively small portion of salaries above $20 million.

I also made a set of density plots for the offensive and defensive performance stats to understand the distributions of player metrics. The distributions for each stat vary in spread but are all generally unimodal. Most of the distributions are skewed right like the salary distribution, but free throw percentage and three point percentage are slightly skewed left.

Similar to the offensive distributions, the distributions of defensive stats vary noticeably. Rebounds per game and steals per game have relatively wide distributions while steal-turnover ratio and blocks per game have much tighter distributions.

My next step was to plot the interactions between player salaries and different variables that I predicted would be important for estimating player salary. Based on the scatter plots, points per game has the strongest correlation with player salary. Rebounds per game and steals per game look to have a weak corrlation with salary while 3pt percentage, assist-turnover ratio, and blocks per game do not look to have a significant connection to player salary.

I made a correlation heatmap with seaborn to visualize the interactions between variables. I was particularly interested in how other variables are correlated with salary. The metrics that proved to be most correlated with a player's salary were minutes per game, assists per game, rebounds per game, steals per game, and especially points per game. Blocks per game are seemingly valued less than these other stats, as they had only roughly half as high of a correlation with salary compared to metrics like steals or rebounds per game. This heatmap also shows some other notable correlations, such as the strong correlation between minutes per game and other game-level stats such as points per game.


### EDA Images

Salary Density Plot:

![NBA Salary Density Plot](Images/nba_salary_density.png)

Scatterplots with relationship between salary and performance statistics:

![Relationship Scatterplots](Images/nba_scatters.png)

Correlation Heatmap:

![Correlation Heatmap](Images/nba_heatmap.png)


---

## Modeling



---

## Results



---

## How to Run

Clone the repository:

```bash
git clone https://https://github.com/a-g-mikulsky/NBA-Salary-and-Performance-Analysis-2025-2026.git
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

## Author

Aiden Mikulsky | Data science student at the University of Wisconsin-Madison

- LinkedIn: www.linkedin.com/in/aiden-mikulsky-ab226a337
- Email: aidenmiku920@icloud.com
