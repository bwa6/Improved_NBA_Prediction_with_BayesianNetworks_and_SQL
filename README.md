# Improved_NBA_Prediction_with_BayesianNetworks_and_SQL
Pipeline for predicting player statistics in a game using Bayesian NNs with data stored in a SQL database

In this project, I am improving upon my previous NBA prediction model.  In this version, I input the data into an SQL database.  I createed scripts to manipulate data into a training set with the ability to tune how the training set is established.  Variables include:

1. Whether to include playoff games
2. How many minutes a player has to average per game to be included (generally only interested in making predictions for key players)
3. How many recent games to average when calculating average stats per game leading up to the game of interest
4. How many days to skip from the beginning of the regular season to make sure there are enough games to calculate averages
5. How mnay of the most recent seasons should be included

I can use these variables to perform a gridsearch to find the combination that generates the most accurate model.  For instance, is it better to include averages over the last 1,2,3... games to get the best predictions.  How does the addition of playoff games affect predictions for regular season games?  In my previous project, I only used 2021 regular season data to make predictions for the 2021 playoffs.  Unsurprisingly, my predictions were lower than the true values since players tend to try much harder in the playoffs and score more.  Thus, I will overcome that limitation in this project.  In addition, I will consider more than just one season.  The game evolves rapidly over time so data from 10 years ago may not be predictive anymore.  Therefore, I will determine how many seasons I should include to get the best predictions.  Ideally, I want to utilize asmuch data as possible aslong as it is still predictive of recent games.
