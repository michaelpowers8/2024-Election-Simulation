# 2024-Election-Simulation
With the 2024 election heating up, I have decided to create a simulation of how the 2024 election were to play out if it were held on July 8, 2024. All numbers calculated in this simulation were from poll numbers found on [538](https://projects.fivethirtyeight.com/polls/president-general/2024/), [CNN](https://www.cnn.com/election/2024), [New York Times](https://www.nytimes.com/interactive/2024/us/elections/polls-president.html), [Fox News](https://www.foxnews.com/elections/2024/primary-results/voter-analysis), and [270 to Win](https://www.270towin.com/2024-presidential-election-polls/). This simulation will be updated periodically. Here is how the simulation is orchestrated.
## Global Final Variables
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

## Global Dynamic Variables
Here are the variables that will change as the simulations proceed:
1. state_polls
    * Dictionary where the key is the name of a state and the item is a list of popularity based on polls where the first item is Donald Trump popularity and the second item is Joe Biden popularity. There is also an error added to each candidates popularity for polling errors that occur. That error for each candidate will fluctuate from integers -4 to +4. For example: Wisconsin: [46,47] is the baseline polling, but if you have an error for Trump of -3 and an error for Biden of -1, the simulation will evaluate as if it wrote Wisconsin: [43,46]. That will produce one set of results and errors of +2 for Trump and -1 for Biden will produce different results. 
