# Calculating the Pythagorean Expectation for the Premier League
The Pythagorean Expectation is a classic approach to analyzing the performance of teams in North American sports, first developed for Major League Baseball by STATS INC. in the form

```
\text{Win Ratio} = \frac{(\text{Runs For})^{k}}{(\text{Runs For})^{k} + (\text{Runs Against})^{k}}
```

where the k-value changes to account for the different scoring data of each sport (MLB: 1.83, NBA: 13.91). The premise of the Pythagorean Expectation is that it provides an accurate model for the expected win-percentage of a team, thus allowing one to compare the expected wins vs. the actual wins and analyze the over or under-performance of a team. This project seeks to adapt the Pythagorean Expectation to the Premier League.

## Rationale
Using similar existing calculations as used in North American sport with goals for and goals against substituted for runs for and runs against, it is possible to find the unique k value for the Premier League to find a rough estimate of the amount of points a team should be achieving each season. However, the most daunting challenge to fitting the Pythagorean Expectation to the Premier League comes from the unique nature of the sport: wins worth 3 points, draws worth 1 point, and losses worth none. The original Pythagorean Expectation does not account for the possibility of a draw, so it models only win-percentage. To account for the point-system and three-outcome nature of European football, it is necessary to calculate points-percentage rather than win-percentage, i.e. the percent of the total available points that a team should be expected to earn. Thus, the formula for this Pythagorean model becomes 

```
\text{Points Percentage} = \frac{(\text{Goals For})^{k}}{(\text{Goals Against})^{k} + (\text{Goals For})^{k}}
```

## Data
This model uses data from Kaggle over every match result from the Premier League from the 1993-1994 season to the 2024-2025 season. This means that over 12,000 matches and results are analyzed in search for an ideal k-value for the Premier League. 

## Calculations
In order to find the ideal Pythagorean k-value, we use logistic regression. As we are searching for points-share, a percentage value between [0,1], we use a binomial GLM with a logit link to fit the logit of real points-percentage-earned as a linear function of the log of (Goals For/Goals Against) is graphed. 

```
logit(p) = k \log\!\left(\frac{GF}{GA}\right)
```

However, the traditional Pythagorean Expectation formula assumes that a team with an equal amount of goals scored and conceded would have a points-percentage of 50%. However, while this assumption does work for other sports, in the Premier League, a team that earns 50% of the points available (57), should expect to have a positive goal difference and safely be in the upper-half of the league table. As such, an intercept, represented by the variable alpha, is necessary to more accurately account for how goal difference affects points-based performance. 

```
logit(p) = \alpha + k \log\!\left(\frac{GF}{GA}\right)
```

This model finds a k-value for the Premier League of approximately 1.209 and alpha value of -0.167.

## Accuracy
This model has a calculated MAE of 3.587 and RMSE of 4.479, indicating that the model is accurate to a degree of 1-2 games. This represents a stark improvement over current expected-points calculations such as FootyStats' (MAE 9.400 and RMSE 11.885).

Using these statistics for comparison, this model represents a roughly 62% reduction in errors and 2.6x increase in accuracy over existing models. 
