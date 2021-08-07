# Improved_NBA_Prediction_with_BayesianNetworks_and_SQL
Pipeline for predicting player statistics in a game using Bayesian NNs with data stored in a SQL database

In this project, I am improving upon my previous NBA prediction model.  In this version, I scraped data from basketball reference and ESPN into an SQL database.  I createed scripts to manipulate data into a training set with the ability to tune how the training set is established.  Variables include:

1. Whether to include playoff games
2. How many minutes a player has to average per game to be included (generally only interested in making predictions for key players)
3. How many recent games to average when calculating average stats per game leading up to the game of interest
4. How many days to skip from the beginning of the regular season to make sure there are enough games to calculate averages
5. How mnay of the most recent seasons should be included

I can use these variables to perform a gridsearch to find the combination that generates the most accurate model.  For instance, is it better to include averages over the last 1,2,3... games to get the best predictions.  How does the addition of playoff games affect predictions for regular season games?  In my previous project, I only used 2021 regular season data to make predictions for the 2021 playoffs.  Unsurprisingly, my predictions were lower than the true values since players tend to try much harder in the playoffs and score more.  Thus, I will overcome that limitation in this project.  In addition, I will consider more than just one season.  The game evolves rapidly over time so data from 10 years ago may not be predictive anymore.  Therefore, I will determine how many seasons I should include to get the best predictions.  Ideally, I want to utilize asmuch data as possible aslong as it is still predictive of recent games.

I also expanded the number of features.  In my previous project, I did not include features on injury, which is obviously limiting since we would expect dramatic differences if a team's superstar is injured.  To quantify injury, I scraped injury reports from basketball reference and salary data from ESPN.  I calculated how much of a team's total salary is paid to injured players.  Therefore, when highly paid superstars are injured, this value will be really high compared to a low paid role player.

I also considered features based on start time, such as 7:30 or 10:30 pm, and day of the week.  Players may try harder during 'prime time' games.  I also included features for how far into the season the game of interest is.  Perhaps players may try harder near the end of the season when playoff seeding becomes an important consideration.

In my previous project, I considered features based on season averages and recent performance.  However, I decided to get rid of season averages since it would have a different effect depending on whether the game is early or late in the season since season averages would be more accurate later in the season.  Therefore, I will only consider recent performance, which will have a consistent impact regardless of the part of the season.

In my previous project, I also considered the absolute stats of the opposing team.  In this project, I decided to incorporate the team's previous opponents' performances by taking the difference in the stats by the current opponents and the average stats of the opponents the team has played recently.  By utilizing this difference, we can compare the quality of the current opponent to recent opponents.  For example, if the team played low quality teams, a player's recent stats may look inflated.  If the model recognizes that the current opponent is much better than the previous, it may decide to make lower predictions than the player's previous performance would otherwise suggest.

In my previous project, I made predictions using a voting regressor that combined the results of a random forest, support vector machine, and lasso/ridge regression.  In this project, I developed a probabilistic neural network.  The advantage of this is the output is not just a single prediction but a normal distribution.  The prediction would be the mean and the uncertainty is the standard deviation.  As a result, I can sample from this distribution to determine how to wager on a bet of interest.  For each prediction, I sampled from the distribution object 1000 times and compared how many of these were over vs. under the over/under line set by the betting website.  The percent of samples over/under the line represents the predicted probability of winning the bet.  I use this probability in the arbitrary formula to determine how much to bet, which I established in my previous project. 

I continue to improve this project so that it will be ready for making predictions on the 2022 regular season.  However, I have gotten some preliminary results using the over/under data from the 2021 playoffs that I collected during my previous project. The results can be viewed in the NBA_ProbabilisticNN_model.ipynb file.  Unfortunately, I lost money whereas I made a profit using my original model.  However, this is not too concerning since I think it is inappropriate to make predictions on the playoffs since the bulk of the training data is regular season data.  It will be more informative to test the validity of the model when I start to get over/under data on the 2022 regular season.

I will continue to tinker with this model and test the effect of different features to get the most accurate model for betting on the 2022 regular season. Stay tuned for updates on this project.

FILES:

1. database_schematic4.png

This is an image of the schema for the SQL database of data scraped from the web.  Primary keys are highlighted in red.

2. Create_initial_NBA_db_Player_Season_Salary_Team.ipynb

This Jupyter Notebook creates the Player, Season, Salary, and Team tables as shown in the database schematic.  

3. Create_initial_NBA_db_Game_TeamStats_PlayerStats.ipynb

This Jupyter Notebook creates the Game, TeamStats, and PlayerStats tables as shown in the database schematic.  

4. Create_NBA_training_data.ipynb

This Jupyter Notebook creates training data based on the five variables listed above and saves the resulting data frames into the train_data folder.

5. NBA_ProbabilisticNN_model.ipynb

This Jupyter Notebook develops a deep learning model where the output is a distribution object.  The advantage of this is the ability to model the uncertainty in the predictions.  A high standard deviation of the distribution object would mean high uncertainty.  This is valuable information when deciding how much to wager on a particular bet.  I have preliminary results at the end of the Notebook demonstrating performance on the 2021 playoffs.

6. n_seasons5avgGames.... 

CSV files starting with this name represent potential training data for a model.  These files are created using Create_NBA_training_data.ipynb. Each file has a different set of parameters, such as how many games to average to get a player's recent performance, whether to include playoff games, etc.

7. test_n_seasons5avgGames.... 

CSV files starting with this name represent test sets with over/under lines set by a betting website to test the ability of the model to make money by winning bets.

8. NBA_Fantasy_db.sqlite 

This file is not included because it is surpasses Github's memory limitation.  However, you can create this yourself using the code provided.
