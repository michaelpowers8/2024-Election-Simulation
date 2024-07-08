# 2024-Election-Simulation
With the 2024 election heating up, I have decided to create a simulation of how the 2024 election were to play out if it were held on July 8, 2024. All numbers calculated in this simulation were from poll numbers found on fivethirtyeight.com, cnn.com, nytimes.com, foxnews.com, and 270towin.com. This simulation will be updated periodically. Here is how the simulation is orchestrated.
## Global Final Variables
This section contains the 4 global variables that will never change throughout the entirety of this simulation.
1. states
     * List of all 50 states as Strings
2. electoral_votes
     * Dictionary where the key is the name of the state and the item is the number of electoral votes that state holds
3. registered_voters
     * Dictionary where the key is the name of the state and the item is the number of registered voters according to [KFF.org][http://example.com](https://www.kff.org/other/state-indicator/number-of-voters-and-voter-registration-in-thousands-as-a-share-of-the-voter-population/?currentTimeframe=0&sortModel=%7B%22colId%22:%22Location%22,%22sort%22:%22asc%22%7D].
