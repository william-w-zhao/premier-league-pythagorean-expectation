# Calculating the Pythagorean Expectation for the Premier League
The Pythagorean Expectation is a classic approach to analyzing the performance of teams in North American sports. First developed for Major League Baseball by STATS INC. as Win Ratio = (Runs for)^k/((Runs for)^k + (Runs against)^k)) with the k-value of 1.83, it was soon adapted into baseball (k = 13.91) as a valuable tool for judging the expected winning percentage of a certain team, and thus, yielding whether a certain team was over or underperforming. This project seeks to adapt the Pythagorean Expectation to the Premier League, fitting a new equation using points-share instead of win ratio to fit European football's unique system of 3 point wins, 1 point draws, and 0 point losses. 

## Rationale
Using similar existing calculations as used in North American sport with goals for and goals against substituted for runs for and runs against, it is possible to find the unique k value for the Premier League to find a rough estimate of the amount of points a team should be achieving each season. 

## Data
This model uses data from Kaggle over every match result from the Premier League from the 1993-1994 season to the 2024-2025 season. This means that over 12,000 matches are analyzed in search for an ideal k-value for the Premier League. 

## Calculations
In order to find the ideal k-value, we will manipulate the traditional Pythagorean Expectation equation to encapsulate wins, losses, and draws alike. As we are searching for points-share, a percentage value between [0,1], we can use a binomial distribution with a logit link in a logistic regression-based GLM to find k as the slope of the of best fit when the logit of real points-percentage-earned vs. the log of (Goals For/Goals Against) is graphed. 

```math
logit(p) = k \log\!\left(\frac{GF}{GA}\right)
```

However, this traditional interpretation of the Pythagorean Expectation equation would use a simplistic view of how points are earned in the Premier League. For a team with a Goal Difference of 0, the log of Goals For/Goals Against would equal 1, meaning that the expected points-share of the total would be 50%. However, this is not the case: Indeed, most teams that earn 50% of the available points in a Premier League season would score more than they concede. As such, an intercept is necessary to account for how much a team's Goals For and Goals Against truly affect a match. 

```math
logit(p) = \alpha + k \log\!\left(\frac{GF}{GA}\right)
```

## Accuracy
The model has a mean absolute error of 3.587, approximately the points in 1 win. Furthermore, the bias, -0.00779, is negligible. The model is both strongly predictive of the points that a Premier League team should achieve in a season purely based on Goals For and Goals Against, while also being centered with little bias. 

## Conclusion
This model finds a k-value for the Premier League of approximately 1.209. This means that for each time a team doubles their ratio of Goals For/Goals Against, they should expect that their points would increase by approximately 2.311. Here are some interesting takes from recent seasons of the Premier League: 

In 2024-2025, Liverpool and Arsenal were only separated by a mere point in expected points (xP) (76.866 vs. 75.859). However, Liverpool exceeded their xP while Arsenal disappointed, leading to the romp to the title for Liverpool. 

In 2023-2024, Arsenal should've won the league with a xP nearly 2 higher than Manchester City (87.905 vs. 85.249). However, a large overperformance from Manchester City led to their title win. 

In 2022-2023, Manchester City should've won the league significantly, with a xP that outstripped Arsenal's second place by nearly 9 points (85.477 vs. 76.116). In this season, the tables were flipped, with a large overperformance from Arsenal. 
