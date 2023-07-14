# Who can replace Bruce Brown for the Nuggets? NBA Player Similarity
<img src="https://github.com/segravjf/nba_player_similarity/assets/13812785/32df255b-31d9-4491-8f1b-e9d4bb659fd8" alt="Nuggets winning the championship" style="width:600px;"/>

They finally did it. After more than 50 years of existence, the Denver Nuggets won their first ever NBA Championship. I couldn't believe my eyes. The Nuggets were probably one of the first sport teams I remember rooting for, basking in live games featuring Alphonso Ellis and Dikembe Mutombo. As a kid barely of reading age, I remember eagerly opening up the newspaper every fall and winter morning to see if a new box score was available for me to peruse. And finally, they climbed that apex and did it.

There were celebrations in the streets, parades through downtown Denver, odes to a fan base that had been long suffering and building a team the "right way", and assurances they weren't done yet.

And then it was time to think about next season.

<img src="https://github.com/segravjf/nba_player_similarity/assets/13812785/0774cacb-a9a0-46cf-9add-d8d14268a798" alt="Bruce Brown holding the NBA championship trophy" style="width:300px;"/>

A player who caught my eye during this championship run wasn't the biggest star (Nikola Jokic), the players who overcame traumatic injuries (Jamal Murray, Michael Porter Jr.), or the physical workhorse (Aaron Gordon). I was pleasantly surprised all year by Bruce Brown, a summer pickup who signed for a modest contract to anchor Denver's second line. By the end of the campaign, BB was slotting in with other starters, mixing his ballhandling and cutting skills off the dribble on offense with shutdown defense for multiple position groups.

## The problem
Following the end of the season, Brown opted out of his contract's second year, putting him (deservedly) on the market again to get a bag in free agency. It's the right move for him, but the Nuggets also need to react appropriately. To make a championship winner turn into a dynasty, teams need both a healthy core with continuity and shrewd additions in free agency that can play in their orbit.

While there will never be anyone with precisely the skillset of Bruce Brown for the 2023-24 Denver Nuggets, who should they target to bring in as a free agent to fill in his responsibilities?

Analytics can help us in this venture.

## The methodology
We can use a variety of statistics, including shooting, (possession-adjusted) counting stats, and advanced metrics widely used by coaching and front offices across the NBA to understand what Bruce Brown does well, and subsequently identify other players who are similar to his skillset. 

[Cosine similarity](https://builtin.com/machine-learning/cosine-similarity) is a technique used mostly in text analytics capacities to measure the similarity of different documents. Its advantages are numerous, including needing relatively simplistic regularization (such as z-scores) to work well, easy handling of null cases (which are often a problem for advanced stats that may require certain participation levels to qualify), and straightforward interpretation (100 is great, 0 is awful).

I was initially inspired by a similar application in soccer/football by Ben Griffis on Twitter, where he used Cosine Similarity as the [basis](https://github.com/griffisben/griffis_soccer_analysis) for a player similarity score across different football leagues. It's also [widely used](https://statsbomb.com/articles/soccer/doppelgangers-finding-similar-players). by sports analytics consultancies like StatsBomb to measure player similarity. Provided we take the right metrics into consideration, it should be pretty easy to get a match going.

A key piece of this analysis is not to throw the entire kitchen sink of stats at this algorithm and let it cook, but to focus specifically on *stylistic* indicators over *performance* indicators. While some stats can be used for both use cases (such as usage rate, some adjusted field goal stats, etc.), we wouldn't want to include holistic grade stats like PER, RAPTOR, and the like. They should instead be used as dimensions to filter down candidates.

My primary inspiration for which stats to use came from past research around NBA player archetypes. [This clustering analysis](https://global-uploads.webflow.com/5f1af76ed86d6771ad48324b/5f6a65517f9440891b8e35d0_Kalman_NBA_Line_up_Analysis.pdf) shared at the Sloan Sports Conference 2019 by Kalman & Bosch created segments of basketball players, which isn't unlike similarity, and identified a broad set of stats that Basketball Reference already made available. I decided to use that as the basis for the similarity vectors.
![image](https://github.com/segravjf/nba_player_similarity/assets/13812785/1ba539a6-90ad-4d63-a844-3c5fbce10037)

## The data
The NBA is replete with strong data sources. I initially used the [NBA API](https://github.com/swar/nba_api) to gather some basic stats for all players, but then decided it was actually easier to download the data of interest from [Basketball Reference](https://www.basketball-reference.com/), which has convenient datasets available for [advanced](https://www.basketball-reference.com/leagues/NBA_2023_advanced.html), [shooting](https://www.basketball-reference.com/leagues/NBA_2023_shooting.html), and [per possession](https://www.basketball-reference.com/leagues/NBA_2023_per_poss.html) for all players in an easily downloadable format. While the NBA API has more granular data available, I think I could piece together the profiles I want from the Basketball Ref data alone. I downloaded the datasets to CSV and imported them directly for use in the analysis.

## Results
While there is a lot more detail in the notebook, there were some fascinating early results:

```python
## Find similar players to Bruce Brown
bruce_sims = find_similar_players(stats_raw, 'Bruce Brown', ['PG','SG','SF'])

## Preview similar players
bruce_sims.head(20)
```
<img width="836" alt="image" src="https://github.com/segravjf/nba_player_similarity/assets/13812785/00f4f32b-53cd-4cd3-979a-472f3af91a36">

Based on the early results from the similarity score, two players that Denver also features on their bench, Christian Braun and Peyton Watson, already score as highly similar players to Brown based on these stats.

While more analysis is needed to make definitive conclusions, this gives me a hypothesis that there may exist something of a "Denver system" that role players can plug into, which somewhat allays my fears of an unfillable talent hole caused by Brown's departure.

We also seem to see a bevy of other higher-usage bench guys like your McDanielses and your Terance Manns. 


## What's next?
This analysis is not yet finished, as I'm hoping to import some contract data to further filter down the available signees the Nuggets could make. This includes:
* Filter down top matches to only those players who are available to sign (and ideally can fit into the Nuggets' cap situation).
* Do more to validate the algorithm against other use cases, perhaps by testing known play style doppelgangers.
* Analyze the most similar players to BB against catchall output metrics to understand not only similarities, but outputs, to guide the Nuggets' decision making.

In addition, we could look at further enhancements to the similarity score by adding or replacing existing stats:
* Rather than using catchall stats for 2 point and 3 point shots taken, use more specific ranges to differentiate layups, floaters, midrange jumpers, etc. to find those who not only take similar shot volumes of each to BB, but also those who like to frequent the same areas of the floor for their shots.
* Find a better proxy for both "cut and slash" behavior (a big feature of Denver offenses that BB excelled at) and % of time defending key offensive players (another responsibility that BB took on).
* Think about a better way to limit similar players than positions, which are pretty fluid.
