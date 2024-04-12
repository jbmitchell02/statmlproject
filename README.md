# StatML Final Project
Clayton Walther, Mitch Mitchell, Waleed Akram

For our final project, we’re interested in investigating sports betting, which has grown into a billion dollar industry since the 2018 U.S. Supreme Court ruling on Murphy v. National Collegiate Athletic Association. Bettors place bets with sportsbooks, who give them odds that dictate the amount of money the bets will payout if successful. Sportsbooks attempt to set these odds such that their bettor payouts are balanced across all of the potential outcomes, which mitigates their risk by making their profit indifferent to the outcome. So, their odds typically reflect bettor sentiment rather than the probability of the outcome occurring. However, bettors would generally prefer odds that reflect the probability of the outcome occurring as to be able to place more informed bets. This motivates the main goal of our project, which is to develop models for predicting the probability of bet outcomes that are better calibrated than the implied probabilities from the odds provided by sportsbooks.

We plan on focusing on NBA games and their three primary bet types:
* moneyline – the winner of the game,
* spread – whether the spread between the team’s scores is over/under a given threshold,
* total – whether the total number of points scored is over/under a given threshold.

All three of these bet types are closely related to the number of points a team scores, so we will begin by using regression analysis to identify a set of predictors that are correlated with the number of points a team scores in a game. Then, we will proceed by using subset selection and shrinkage methods to select the best predictors for each bet type. Since we’re interested in predicting probabilities, logistic regression stands out as our main model choice, especially for moneyline bets. However, we also plan to consider linear discriminant analysis, tree-based models, and other models that may arise from our literature review. Though not covered in the course, we also may consider Bayesian linear regression for spread and total bets. This is because the over/under thresholds aren’t the same for every game, so a model that can predict probabilities for different thresholds without refitting could be useful.

Our objectives are as follows:
1. Review related academic literature to identify the models and predictors that have been successful in predicting outcomes related to basketball games.
2. Perform regression analysis against the points a team scores in a game to identify correlated predictors.
3. Develop predictive models for the probability of outcomes for moneyline, spread, and total bet types.
4. Analyze the predictive capability and calibration of our models in comparison to the implied probabilities from the odds provided by sportsbooks.

As for team roles, we intend to contribute equally towards the first two objectives. For the last two objectives, we will each focus on developing and analyzing models for one bet type:
* moneyline – Waleed,
* spread – Mitch,
* total – Clayton.
