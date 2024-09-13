# NBA_Draft_Optimization_as_a_Knapsack_Problem
This repository contains the data and model used to optamize the first 17 picks of the 2022 NBA Draft by modeling a simple linear Knapsack problem using the pulp Python package. I created this model for a group project in Northwestern's MSDS 460 Decision Analytics course.

The data source for this analysis comes from the official NBA website. The downloaded file contains comprehensive college statistics for players drafted in 2022. We determined that the most relevant metrics for assessing player quality were field goal percentage (FG%), points per game (PPG), rebounds per game (RPG), and assists per game (APG).

To quantify player performance, I created a "Player Rating" (PR) metric, which incorporates these key statistics from the player's most recent college season. The formula used is as follows: 

Player Rating = (PPG*FG%) + RPG + APG.

This equation is designed to provide a balanced assessment of a player's offensive efficiency (PPG *FG%), rebounding ability (RPG), and playmaking skills (APG). By multiplying points per game by field goal percentage, we emphasize scoring efficiency, ensuring that players who score frequently and effectively are rated higher. Adding rebounds and assists per game ensures that all-around performance is taken into account, not just scoring.

In addition to the Player Rating, I introduced a variable called "Position Quant" to account for team-specific needs in our model. Each team was assigned a weight based on their positional demands: Center (C), Forward (F), or Guard (G). The weights were determined as follows: 

2 for the most critical position a team needs to fill.
1.5 for the secondary position of importance.
1 for the least critical position.

These weights were assigned based on each team's roster needs. Access to each teams, official internal needs was not something we had access to, therefore we had to assign these values based on a group members knowledge of the sport. This Ensures that the model not only identifies the best available talent but also addresses team-specific strategic needs. The file “2022_Draft_Weights.csv” consolidates this information. The combined use of Player Rating and Position Quant allows us to model draft scenarios effectively, addressing both player quality and team needs comprehensively.

I used these described weights to create an objective function for each of the first 17 teams in the draft. This function is a summation of the   products of the two weights and the integer variable xi, for each player:

∑ 𝑃𝑙𝑎𝑦𝑒𝑟 𝑅𝑎𝑡𝑖𝑛𝑔𝑖 · 𝑃𝑜𝑠𝑖𝑡𝑖𝑜𝑛 𝑄𝑢𝑎𝑛𝑡𝑖 · 𝑥𝑖   For players in the draft i.

Each team's objective function is successively maximized subject to the constraint that the sum of all integer variables xi cannot exceed 1, thereby ensuring that only one player is chosen by a team during an individual pick. Additionally, once a pick is made, the list of available players is updated to reflect the change in available players for the next team, thereby ensuring that the model optimizes for only the available players in the current pick.
