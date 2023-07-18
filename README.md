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
The cosine similarity exercise outputted some fascinating early results before filtering to actually available players: 

```python
## Find similar players to Bruce Brown
bruce_sims = find_similar_players(stats_raw, 'Bruce Brown', ['PG','SG','SF'])

## Preview similar players
bruce_sims.head(20)
```
<img width="836" alt="image" src="https://github.com/segravjf/nba_player_similarity/assets/13812785/00f4f32b-53cd-4cd3-979a-472f3af91a36">

Based on the early results from the similarity score, two players that Denver also features on their bench, Christian Braun and Peyton Watson, already score as highly similar players to Brown based on these stats.

While more analysis is needed to make definitive conclusions, this gives me a hypothesis that there may exist something of a "Denver system" that role players can plug into, which somewhat allays my fears of an unfillable talent hole caused by Brown's departure.

We also seem to see a bevy of other higher-usage bench guys like your McDanielses and your Terance Manns. Including usage rate in the similarity limits existing big-name stars from being included as high scoring comps to match the role that Brown inhabits for the Nuggets. This is a tradeoff of that inclusion: high usage players also tend to be quite talented players, as there aren't a lot of situations in the league where efficient scorers purposefully take less time on the ball. The Nuggets are one of those places, but they also have some cap difficulties due to those other high earners in the starting lineup. Brown coming to Denver on a "prove it" deal was an impressive coup for both player and team that will be hard to replicate.

Looking at specific comps who are available shows some of that difficulty:
![image](https://github.com/segravjf/nba_player_similarity/assets/13812785/694a45c4-5854-4b08-9fd7-150491a4a3d2)

The players listed here sit at the middle of the Venn diagram of (1) availability, (2) similarity, and (3) output. None of the players in this comp group matched the overall output of Brown across most all-in-one metrics of value (PER of 13, WS/48 of 0.09, VORP of 0.9; all pretty solid for the minutes and limited role he was alotted). 

From the initial list, we winnowed the group to 14 finalists:

<img width="218" alt="image" src="https://github.com/segravjf/nba_player_similarity/assets/13812785/150a845b-2392-49f7-b70d-2c82aadc2984">


Going one level deeper, we assessed each of these finalists against key dimensions that BB brought to the 2022-23 Nuggs:
* Defense (these are admittedly not the best catchall metrics of defensive value, but BB did score well on all of these dimensions)
  ![image](https://github.com/segravjf/nba_player_similarity/assets/13812785/ff435df1-fd5b-45b3-91b2-fed835c98d96)
  ![image](https://github.com/segravjf/nba_player_similarity/assets/13812785/7ac29921-677d-48e9-858f-732af169bd3d)
* Shooting (the dimension that BB was weakest over the regular season)
  ![image](https://github.com/segravjf/nba_player_similarity/assets/13812785/2f61ec76-22cd-42ab-b89a-a457469f3cd4)
* And playmaking/ballhandling

  ![image](https://github.com/segravjf/nba_player_similarity/assets/13812785/20e8f483-772a-47ef-a67a-84c7a1aae3e9)

Putting these profiles together, we can see some players who contribute more to each dimension than others. We can illustrate that here:
![image](https://github.com/segravjf/nba_player_similarity/assets/13812785/7abc8746-c355-4ce0-9e3e-e9c550f72adc)

So who is the ideal Bruce Brown sixth man replacement for the Nuggets?
* **Hamidou Dially** was one of the few players who scored more efficiently than BB while being highly similar in style, while not "excelling" at any one dimension. What he lacks in 3P shooting, he makes up for in overall scoring efficiency, largely from driving, while still showing strong defense in a limited (injury-addled) role.
* **Javonte Green** checked the boxes on shooting and defense, though his absolute output and usage could raise some flags. He hews closer to a small-ball forward contributor, but played plenty of SG as well. If the Nuggets are comfortable with Reggie Jackson owning ballhandling when Jamal Murray is out, he could slot in well as a complementary scorer.
* **Dennis Smith Jr.** is an interesting veteran option, more of a pure PG with strong playmaking and defensive contributions. While he hasn't been an efficient scorer, a more specialized role in Denver focused on his strengths could raise those figures.
* The most likely outcome is a growth in minutes for existing teammates **Christian Braun** and **Peyton Watson**, potentially leveraging them differently based on matchups.

In all, this question is a luxury for a championship team whose window to win it all again is still wide open. With all 5 starters coming back, the Nuggets have a whole season to get their sixth man situation solved.


## What's next?
This analysis is not comprehensive -- a lot of the metrics choices were made out of convenience and availability of key metrics, while glazing over some of what Bruce Brown did best (probably something that merits its own section in this analysis). In particular, with another week or so, I would investigate some of these tweaks:
* Rather than using catchall stats for 2 point and 3 point shots taken, use more specific ranges to differentiate layups, floaters, midrange jumpers, etc. to find those who not only take similar shot volumes of each to BB, but also those who like to frequent the same areas of the floor for their shots. In addition, breaking out shots by self-generated vs. catch-and-shoot gets more to the style of shooting than flat-out accuracy.
* Find a better proxy for both "cut and slash" behavior (a big feature of Denver offenses that BB excelled at) and % of time defending key offensive players (another responsibility that BB took on).
* Think about a better way to limit similar players than positions, which are pretty fluid.
* Finally, a more rigorous breakdown of the similarity metric I crafted could go a long way to ensuring its validity in making some of these decisions, as well as a way to quickly "scorecard" why one player matches well with BB for better sharing.
