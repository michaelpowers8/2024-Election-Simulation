# 2024-Election-Simulation
With the 2024 election heating up, I have decided to create a simulation of how the 2024 election were to play out if it were held on July 8, 2024. All numbers calculated in this simulation were from poll numbers found on [538](https://projects.fivethirtyeight.com/polls/president-general/2024/), [CNN](https://www.cnn.com/election/2024), [New York Times](https://www.nytimes.com/interactive/2024/us/elections/polls-president.html), [Fox News](https://www.foxnews.com/elections/2024/primary-results/voter-analysis), and [270 to Win](https://www.270towin.com/2024-presidential-election-polls/). This simulation will be updated periodically. Here is how the simulation is orchestrated. 
## SUMMARY
This simulation is meant to remain objective and go off of polling results that are fairly distributed from all sides of the political spectrum. Some polling numbers I inputted may favor one candidate more than the other at times. However, this simulation is run through 810 times. Here is how it works in general. Read below for details. Each state is assigned a polling number for each candidate with an added error that could occur from a lack of turnout, or a surplus, plus potential voters switching their vote at the last minute. Every registered voter in each state is given the option to vote one at a time. Each voter is assigned a candidate when they reach the front of the line, and then they are given a 80% chance to cast a ballot to their candidate. This is due to the fact that the voter turnout is roughly 80% for presidential elections. After all registered voters in the state have voted, the votes are counted and the results are saved. This process is done for all 50 states plus the Disctrict of Columbia. However, there is another catch, 99% of the time, a voter that is casting their ballot will cast for their chosen candidate. There is also a 0.7% chance they switch to the other major candidate, and a 0.3% chance they choose to vote for the third party candidate at the last second.  Once all 50 states are complete, a final tally of all popular votes and electoral votes are counted and a winner is decided. All results are added to excel files. Here is how this simulation is completed 810 times. Each candidate in state polls is assigned a base value with an added error for last minute changes or turnout. This error ranges from all integers from -4 to +4 inclusive and including 0. Each candidates error is added at the beginning of the simulation independently and then the simulation proceeds. The simulation for each set of errors is repeated a total of 10 times. Each candidate has 9 unique error values and each error set is repeated 10 times. The total is $`9*9*10=810`$.
## DETAILS
### Global Final Variables
This section contains the 4 global variables that will never change throughout the entirety of this simulation.
1. states
     * List of all 50 states as Strings
2. electoral_votes
     * Dictionary where the key is the name of the state and the item is the number of electoral votes that state holds
3. registered_voters
     * Dictionary where the key is the name of the state and the item is the number of registered voters according to [KFF](https://www.kff.org/other/state-indicator/number-of-voters-and-voter-registration-in-thousands-as-a-share-of-the-voter-population/?currentTimeframe=0&sortModel=%7B%22colId%22:%22Location%22,%22sort%22:%22asc%22%7D)
4. candidates
     * List of the two major party candidates currently: Donald Trump and Joe Biden
5. voter_turnout
     * Integer indicating percent turnout for voters. It is currently set to 75 indicating 75% of registered voters will actually cast a ballot for their respective candidate.

### Global Dynamic Variables
Here are the variables that will change as the simulations proceed:
1. state_polls
    * Dictionary where the key is the name of a state and the item is a list of popularity based on polls where the first item is Donald Trump popularity and the second item is Joe Biden popularity. There is also an error added to each candidates popularity for polling errors that occur. That error for each candidate will fluctuate from integers -4 to +4. For example: Wisconsin: [46,47] is the baseline polling, but if you have an error for Trump of -3 and an error for Biden of -1, the simulation will evaluate as if it wrote Wisconsin: [43,46]. That will produce one set of results and errors of +2 for Trump and -1 for Biden will produce different results. 
2. trump_states_won, biden_states_won
    * List of states each candidate has won
3. electoral_votes_results, popular_vote_results, state_vote_counts
    * Dictionaries indicating how many electoral votes each candidate earned, total popular votes each candidate earned, and number of votes earned in each state
4. combined_states_results
    * List of DataFrames showing the details of every state, who won, how many votes did each candidate get, what percent of the vote did they receive, and how many electoral votes were casted. Each simulation has reported results that are added into a DataFrame and then added into this list of DataFrames. Once all election simulations are complete, further analysis will be completed on this.
5. combined_national_results
    * List of DataFrames showing the summarized national results of the simulated election. Each DataFrame contains how many electoral votes each candidate earned, how many total votes each candidate received nationwide, and what percent of the popular vote each candidate received.
6. election_scores
    * DataFrame providing a very quick way to see who won each simulated election with errors for each candidate.
### Functions
1. format_with_commas
    * Convert all numbers in DataFrames to numbers with commas to make them easier to read.
2. set_state_polls
    * Set the variable state_polls to the baseline value plus trump error and biden error. This function creates variability and accounts for errors when constructing public polls.
3. reset_variables
    * Resets all dynamic variables to initial values after a full election of all 50 states plus District of Columbia is completed.
4. get_individual_state_votes
    * Print the current state that votes are being casted and counted. Set trump_state_votes and biden_state_votes to 0. Loop through all registered voters in the state. Assign a voter their preferred candidate based on a randint between 1 and the sum of the current state polling numbers. Assign the voter an actively voting number between 1 and 100. Check if the voter active status is less than or equal to 80. If yes, they will cast a ballot. If their choice is less than or equal to Trump's popularity. If yes, Trump's state vote, popular vote, and state total votes count increase by 1. If the voter's choice is greater than Trump's polling popularity and less than the sum of popularity between Trump and Biden, Joe Biden's state vote, popular vote, and state total vote is increased by 1. If the voter's choice is greater than the sum of the popularity of both candidates, the third party's popular vote and state vote is increased by 1. After all voters have completed, the votes are tallied. If Trump has more votes, his electoral votes increase by the number of electoral votes the state has and Trump's states won list adds the current state to his list. If Biden has more votes, then Biden's electoral votes increases by the number of electoral votes the state has and Biden's states won list adds the current state to his list. If the vote count is a tie, a new vote is casted between the two candidates.
5. get_state_results_df
    * Creating the DataFrame to display the details of each state in this simulated election. The DataFrame contains 7 columns:
        - State: The name of the state
        - Electoral_Votes: The number of electoral votes the state offers
        - Winner: The name of the winning candidate
        - Trump_Vote_Count: The number of votes Donald Trump earned in the state
        - Biden_Vote_Count: The number of votes Joe Biden earned in this state
        - Trump_Vote_Percent: The percentage of votes Donald Trump earned in this state
        - Biden_Vote_Percent: The percentage of votes Joe Biden earned in this state
6. get_national_results_df
    * Returns 2 items in a list that display slightly different summaries of the data
        - national_results_df is a DataFrame with 6 columns
            * Trump_Electoral_Votes: The total number of electoral votes Donald Trump earned in this simulated election
            * Biden_Electoral_Votes: The total number of electoral votes Joe Biden earned in this simulated election
            * Trump_Total_Votes: The total number of votes that Donald Trump earned nationwide in this simulated election
            * Biden_Total_Votes: The total number of votes that Joe Biden earned nationwide in this simulated election
            * Trump_Vote_Percent: The percentage of votes Donald Trump earned in nationwide in this simulated election
            * Biden_Vote_Percent: The percentage of votes Joe Biden earned in this simulated election
            * This DataFrame is added to the combined_national_results list and then reset to None after each simulated election
        - election_scores is a DataFrame with 6 columns
            * Winner: The winner of the simulated election
            * Trump_Electoral_Votes: The total number of electoral votes Donald Trump earned in this simulated election
            * Biden_Electoral_Votes: The total number of electoral votes Joe Biden earned in this simulated election
            * Trump_Polling_Error: Donald Trump's polling error made in this simulated election
            * Biden_Polling_Error: Joe Biden's polling error made in this simulated election
            * Round_Number: The iteration number of this election cycle. Each set of polling errors for each candidate is repeated 10 times.
### Conclusion
The simulation is currently in progress as of July 8, 2024. The entire simulation should take about 3 days. At that point, all recorded data will be uploaded. For now, here is just a start of the election_scores as a csv file. Email me at <micpowers98@gmail.com> and send me feedback on how this could be improved. Keep in mind, the goal of this simulation was to remain 100% objective. There is a lot of controversy out there and trust in journalism and the justice departments is shaky at best on all sides. I want this to create a neutral environment where discussions about the results can be made and remain civil.
