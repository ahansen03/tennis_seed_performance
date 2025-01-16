# Professional Tennis Seed Upset Analysis (2000-2024)

## Background and Project Overview

### What is an "upset"?

In professional tennis, an "upset" is when a player that is expected to defeat their opponent easily ends up losing the match. A few notable examples of this are Roger Federer's loss to Sergiy Stakhovsky in Wimbledon 2013, Martina Navratilova's loss to Kathleen Horvath in the French Open 1983, and Rafael Nadal's loss to Robin Soderling in the French Open 2009. Every tournament one can expect to see at least one upset, if not a few. 

### Tournament Values and Tours

All of the examples of upsets that I just mentioned occurred during a Grand Slam, which is the highest value of professional tournament (2000 points granted to the winner's ranking). Other tournament values include 250s, 500s, and 1000s, whose value corresponds to the amount of points the winner receives. The Olympic Games, which do not currently grant points to the winner, are played every four years. There are also the "Finals", where the top ranked player at the end of the year play a tournament amongst themselves for upwards of 1500 ranking points. Typically, the Grand Slam and 1000 level tournaments have every healthy player trying to play them, whereas the others have a weaker entry list (fewer highly ranked players play them). The top 8/16/32 players (depending on the tournament value) in a tournament by ranking are called "seeds", and are usually granted a favorable draw, meaning they are positioned in the draw (or bracket) away from the other seeds and would not have to face another seed until the later rounds of the tournament.

The professional tennis circuit is divided into two "tours": the WTA (or women's) tour and the ATP (or men's) tour. The main difference between the tours in terms of format is that the ATP tour plays best-of-five sets for all matches in a Grand Slam, whereas every other match is a best-of-three sets format on both tours. This means that in a Grand Slam on the ATP tour, a player has to win three sets to win a match rather than the standard two. Otherwise, the two tours play many of the same tournaments, with the same tournament values, and a very similar schedule.

### Motivation nad Purpose

As someone who spends a lot of time following professional tennis year-round, I often hear the sentiment that the players on the WTA tour are less consistent than the players on the ATP tour. This rhetoric is frequently heard after every surprise exit of a top ranked WTA player in a tournament. This is often followed up with discourse around the differences between women and men physically and emotionally, which can be counterproductive and unfair. 

I aimed to analyze whether the WTA is more inconsistent than the ATP through statistical evidence. I will do this using supporting dashboard visuals and statistical analysis to compare upset frequencies and the variance in upsets. On top of this, I wanted to explore how the difference in match format may be more of a contributor than the physical and emotional differences between the players. Finally, as a tennis fan, I found it would be interesting to touch on how the different surface types (carpet, clay, grass, and hard courts) relate to the frequency of upsets, as the surface type affects the speed and bounce of the ball, potentially favoring certain playstyles and increasing the likelihood of upsets.

Furthermore, I believe my findings here are relevant to the discussion of whether the WTA and ATP should adopt the same Grand Slam format. This is a belief that has the support of many, most notably the founder of the WTA, Billie Jean King. King stated, "I think the women should play 3 out of 5 sets or everyone should play 2 out of 3. But if they play 3 out of 5 sets, then that means they have more content", highlighting how five-set matches create dramatic moments that captivate viewers. Moreover, it would be unfair for the top ranked men to outperform the top ranked women and gain more prize money simply because of the difference in match formats. If my analysis demonstrates this to be the case, that could help justify the need to standardize the Grand Slam match formats.

## Useful Links

These links will all be referenced throughout my report, but they are here as well for easy navigation.

   1. ["Adjusted Seed" VBA Macro Script](./MacroScript): The script used to calculate adjusted seed rankings for players in Excel.
   2. [Raw Data](https://docs.google.com/spreadsheets/d/1Q-vmwI6gNoF-C_xwDSoAolvSXmiG6Af1-6gGYoAU-7I/edit?usp=sharing): The complete dataset from Wikipedia containing player performances and tournament characteristics.
   3. [Google Colaboratory File](https://colab.research.google.com/drive/1GCSkMTTcY_qCbacul9bb1TSKv7E0boAz?usp=sharing): Documentation and intermediate steps of the analysis process.
   4. [Expanded Data Table](https://docs.google.com/spreadsheets/d/1ADziKA-sanMPxB8ABH7TXf9HVaVAK3V8XUj0JIXK_uM/edit?usp=sharing): Our filtered dataset with additional columns, including upset values and player appearances for enhanced analysis.
   5. [Histogram Data Table](https://docs.google.com/spreadsheets/d/1OJNte-aFmfc5PyOiFCanwF82LV0MM0FprKp13VLqOy8/edit?usp=sharing): A simplified table formatted for histogram creation in Power BI, with one line per player.
   6. [Power BI Dashboard](https://app.powerbi.com/reportEmbed?reportId=ebd90410-c30f-43b7-8f37-77e73fdd126f&autoAuth=true&ctid=8b1f75d2-b3e9-4b3d-a9f2-0b5fac458a8d): An interactive dashboard visualizing the analysis results.

## Data Sources + Collection Method

The specific kind of data I sought out was not available in any datasets online, so the first step in this project was to collect it myself. The data was collected from the Wikipedia archives for each professional tennis tennis tournament. These pages list the seeds and how well they performed in that tournament, as well as several fundamental characteristics of the tournament including surface played on, tournament value, and size of draw. 

Originally, I was collecting the data by hand, but this proved to be tedious and unreliable. With the help of AI tools and Python HTML readers such as BeautifulSoup, I was able to automate this process. Since the formatting is often inconsistent between these Wikipedia pages, I then screened the results afterwards to ensure the data was complete and accurate. Afterwards, I created a column which represented wether a player withdrew before the tournament, meaning they didn't play any matches and would not provide useful data. Often a player will do this if they have an unforseen injury or illness.

I wrote a [macro](./MacroScript) in Excel that allowed me to find each player’s *adjusted seed*, or their seed ranking after removing the seeds who withdrew prior to their first match. An adjusted seed value ranks players against only active competitors. The raw data that resulted from this process can be found [here](https://docs.google.com/spreadsheets/d/1Q-vmwI6gNoF-C_xwDSoAolvSXmiG6Af1-6gGYoAU-7I/edit?usp=sharing).

## Identifying Upsets

After collecting the complete dataset, I wanted to create a boolean measure representing whether or not an observation was an upset. Firstly, this meant keeping only seeds with reasonable expectations for higher performance based on tournament context, excluding those where a loss wouldn't conventionally constitute an upset. For example, if the 8 seed in a Grand Slam loses early that would generally be considered an upset, whereas the 8 seed in a 250 level tournament losing early would not be. We call the seeds that remain from this process "upsettable seeds", but for the sake of clarity I will continue to refer to them as "seeds" from now on. 

Without access to historical tennis betting odds, I developed an acceptable metric for what generally indicates an upset occurred. A player in a tournament was considered upset if their performance was more than 2 wins away from reaching their expected rounds won based on their adjusted seed value (for example, the 1 seed is expected to win the tournament and thus all of the rounds there are, etc.). This means that the player significantly underperformed, demonstrating that they could not have been beat by a player who they were not expected to beat based on ranking. The intermediate steps are documented in detail in the [Colaboratory file](https://colab.research.google.com/drive/1GCSkMTTcY_qCbacul9bb1TSKv7E0boAz?usp=sharing). 

To provide additional insights, I added a player appearances column which represents how many observations there are for that particular player in the filtered dataset. The resulting [expanded data set](https://docs.google.com/spreadsheets/d/1ADziKA-sanMPxB8ABH7TXf9HVaVAK3V8XUj0JIXK_uM/edit?usp=sharing) is particularly useful for filtering Power BI visuals. In order to easily make a histogram in Power BI, I also created a [histogram-specific table](https://docs.google.com/spreadsheets/d/1OJNte-aFmfc5PyOiFCanwF82LV0MM0FprKp13VLqOy8/edit?usp=sharing) specifically for that purpose, with one entry per player. Both of these spreadsheets are downloadable through the Colaboratory file or Google Sheets links.

### Note on Tournament Values

Classifying tournaments as Grand Slam, Olympics, or Finals was straightforward. It is worth noting that determining whether a tournament was a 250, 500, or 1000-level required slightly more subjectivity. For this, I referenced both the approximate amount of points granted to the winner of the tournament, the relative frequency of each kind of these tournaments, and the caliber of player that typically signed up for those tournaments.

## Analysis Introduction

I highly recommend exploring the [interactive Power BI dashboard](https://app.powerbi.com/reportEmbed?reportId=ebd90410-c30f-43b7-8f37-77e73fdd126f&autoAuth=true&ctid=8b1f75d2-b3e9-4b3d-a9f2-0b5fac458a8d) that I created and will be referring to for much of this section. There are additional visuals and filters on there that I consider interesting for exploration. The blue and orange colors represent the ATP and WTA tours respectively (note that the colors for the surface visuals represent the surface values). 

I used Welch's t-test for the statistical significance tests, as the sample sizes and variances were often quite different between the groups. The p-values I provide represent the probability of obtaining results as extreme as or more extreme than our observed data, assuming the null hypothesis (that the two measures are the same) is true. Using the standard significance level α = 0.05, I fail to reject the null hypothesis if the p-value is greater than 0.05. Conversely, if the p-value is less than 0.05, I reject the null hypothesis and assert that the two measures are significantly different.

Finally, I will often refer to best-of-five/three sets format as BO5/BO3 for enhanced readability.

## ATP vs WTA Analysis

Our data can be summarized as follows:

<img width="910" alt="Image 1" src="https://github.com/user-attachments/assets/f395ee86-6ec1-41bf-9730-3050c377217b" />

Generally:


| Group  | n | Mean | Variance |
| ------------- | ------------- | ------------- | ------------- |
| WTA  | 6610  | 0.3349 | 0.2228 |
| ATP  | 7042  | 0.3358 | 0.2231 |


With 10+ Appearances:


| Group  | n | Mean | Variance |
| ------------- | ------------- | ------------- | ------------- |
| WTA  | 5856  | 0.3230 | 0.2187 |
| ATP  | 6631  | 0.3276 | 0.2203 |


Looking at players with 10+ appearances allows us to compare the performance of players who were mainstays in the tours, rather than players with only a few appearances as a seed. This gives us a better sense of how the top players in both tours performed. Regardless, the difference in average upset frequency between the ATP and WTA tours in both tables is statistically insignificant (p ≈ 0.91 and 0.58, respectively).

As a result, we cannot make the claim that the two tours perform differently in terms of overall average upset frequency. Furthermore, we can observe from the histograms and tables that the upset frequency variances are approximately equal for both tours. This all culminates in the fact that we observe virtually identical distributions for both tours. This already gives us evidence against the claim that the WTA tour is susceptible to more upsets than the ATP tour, but there are some more interesting conclusions to be found when we break this down even further.

### Match Format

<img width="437" alt="Image 3" src="https://github.com/user-attachments/assets/a3b9407b-1e6a-4360-a540-e491985e0eec" />

| Group  | n | Mean | Variance |
| ------------- | ------------- | ------------- | ------------- |
| Best of 3  | 12068  | 0.3501 | 0.2275 |
| Best of 5  | 1584  | 0.2235 | 0.1736 |


At a statistically significant level (p ≈ 3.3 × 10<sup>-28</sup>), the average upset frequencies between matches that are a BO3 vs BO5 set formats are different. In our sample, BO3 matches have ~35% upset rate, almost 13 percentage points higher than the BO5 matches, which is at ~22%. To compare how the tours do in BO3 matches, we will look at the difference in upset frequency outside of the Grand Slams, which is where the only BO5 matches are played.

### Tournament Value

<img width="427" alt="Image 2" src="https://github.com/user-attachments/assets/9ccaf3f2-e188-48a5-b22c-533b1f28c48a" />

If we look at only the non-Grand Slam data, we observe the following upset frequencies for both tours:

| Group  | n | Mean | Variance |
| ------------- | ------------- | ------------- | ------------- |
| WTA  | 5026  | 0.3613 | 0.2308 |
| ATP  | 5458  | 0.3684 | 0.2327 |

The difference between the means is insignificant (p ≈ 0.45), meaning there is no strong evidence to suggest that upset frequencies differ significantly between the seeds in both the WTA and ATP tours outside of Grand Slams.

Conversely, if we look at only the Grand Slam data, we observe the following upset frequencies for both tours:

| Group  | n | Mean | Variance |
| ------------- | ------------- | ------------- | ------------- |
| WTA  | 1584  | 0.2513 | 0.1882 |
| ATP  | 1584  | 0.2235 | 0.1736 |

Although the means are nearly 3% different, with the WTA having a mean of 0.2513 and the ATP 0.2235, the difference between the means is statistically insignificant (p ≈ 0.06). With our current data we are unable to make the assertion that the seed in both tours perform differently at the Grand Slam level. Intuitively, the shorter BO3 format in WTA Grand Slams would be expected to make upsets more likely, as a lower-ranked player needs to win only two sets rather than three. However, the data does not support this hypothesis statistically.

### Trends over Time

<img width="711" alt="Image 5" src="https://github.com/user-attachments/assets/0cbe60de-d5bf-4022-a41f-72b3ae653188" />

This image visualizes the average upset frequencies each year for both tours, revealing notable trends over time. The time period between 2009 and 2020 is particularly significant, not only because the WTA's overall upset frequency was higher than the ATP's but also because this era saw increasing rhetoric about the perceived inconsistency of the WTA tour. Here is the data for that time period:

| Group  | n | Mean | Variance |
| ------------- | ------------- | ------------- | ------------- |
| WTA  | 3211  | 0.3637 | 0.2315 |
| ATP  | 3028  | 0.3015 | 0.2107 |

The WTA's average upset frequency of 36.37% was significantly higher than the ATP's 30.15% (p ≈ 1.8 × 10-7), suggesting that ATP seeds were more consistent during this period. However, more information is revealed through deeper analysis of the data. By dividing players into two groups based on their total appearances (>= 30 and < 30), we uncover more detailed insights

Players with 30 or more appearances:

| Group  | n | Mean | Variance |
| ------------- | ------------- | ------------- | ------------- |
| WTA  | 2012 (31 players) | 0.3385 | 0.2240 |
| ATP  | 2256 (30 players) | 0.2589 | 0.1919 |

For players with 30 or more appearances, the ATP’s upset frequency (25.89%) was significantly lower than the WTA’s (33.85%) (p ≈ 1.8 × 10-7). This indicates that top ATP players were generally more dominant and consistent than their WTA counterparts during this time.

Players with less than 30 appearances:

| Group  | n | Mean | Variance |
| ------------- | ------------- | ------------- | ------------- |
| WTA  | 1199 (167 players) | 0.4062 | 0.2414 |
| ATP  | 772 (109 players) | 0.4262 | 0.2449 |

For players with fewer than 30 appearances, the difference between tours was insignificant (p ≈ 0.38). Both tours exhibited similar upset frequencies, highlighting that the disparity in overall upset rates was largely driven by the consistency of the most frequently competing players.

#### Impact of Top Players

The dominant performance of the 30 or so players with the most appearances on both tours significantly influenced the overall trends. On the ATP side, the "Big 4" — Novak Djokovic, Roger Federer, Rafael Nadal, and Andy Murray — were particularly dominant. These players had upset rates ranging from 19% to 23% and collectively won 41 of the 47 Grand Slam titles during this period. In contrast, the WTA lacked similarly dominant figures, with a more evenly distributed competitive field.

To illustrate this dominance further, consider the number of unique seeds at 1000 and Grand Slam level tournaments between 2009 and 2020:

   - **Unique Top 3 Seeds**: ATP: 13, WTA: 22
   - **Unique Top 2 Seeds**: ATP: 6, WTA: 19
   - **Unique No. 1 Seeds**: ATP: 4, WTA: 11

This data emphasizes the exceptional consistency of the ATP’s top players to keep their ranking, while the WTA’s top seeds experienced greater variability.

Match formats also play a crucial role in shaping these trends. BO3 matches are more prone to upsets than BO5 matches, and this structural difference influences the tours’ ranking systems. WTA rankings are entirely based on BO3 matches, while ATP rankings are highly impacted by Grand Slam performance, which is BO5 format. Consequently, ATP rankings may better reflect players’ true standings, while WTA players are faced with more variability due to the format.

Despite this difference, the overall upset frequencies for both tours are comparable. This highlights the WTA’s ability to perform at a high level despite facing format challenges, emphasizing that its perceived inconsistency may be overstated.

## Match Surface Analysis

It is well known amongst the tennis community that the surface that a match is played on can have a large effect on a player's performance. While a fast surface like grass courts favor large servers and big hitters, a slower surface like clay favors fast movers who can slide around on the surface and get to the ball quicker. Hard court stands somewhere in between these two in terms of speed. Carpet courts were even faster than grass courts, making matches more volatile and rewarding aggressive, high-risk strategies. However, since 2018 carpet courts have been completely erased from the professional circuit, citing high injury risk as the chief contributor to the decision.

Before looking at upset frequencies for the different surfaces, it is important to note that each court is not used equally, meaning that some surfaces are used more than others. Specifically, tournaments are played on hard courts over half of the time, and then clay courts another ~30% of the time. Grass courts make up the rest of the modern day pro circuit, as carpet courts are no longer used professionally.

<img width="221" alt="Image 6" src="https://github.com/user-attachments/assets/55f8169a-5c8c-4c1e-a945-d0cfe00feb62" />  

Since hard courts are the most common surface for a pro tournament to be held on, one expects that a player's ranking may be more indicative of their ability to play on hard courts over anything else. Accordingly, we might expect the upset frequency on hard courts to be lower. Hard courts provide a more neutral playing field in terms of speed compared to the fast grass courts and the slow clay courts. We can visualize the upset frequencies on each of the four surfaces below.

<img width="271" alt="Image 4" src="https://github.com/user-attachments/assets/540e1836-8047-4484-81b0-3cd842e6dc28" />  

With the exception of the now defunct carpet courts, clay courts have the greatest upset frequency at 36%, compared to grass courts at 34% and then hard courts at 32%. This intuitively makes sense because of the clay surface rewarding players with high endurance while neutralizing power advantages that normally prevail on other surfaces. The difference in means between clay courts and hard/grass courts is statistically significant (p ≈ 0.0008). This statistical difference reinforces the idea that clay courts level the playing field, rewarding stamina and movement over the sheer power that typically prevails on other surfaces.

This analysis emphasizes how different playing surfaces contribute to upset rates. It also explores how rankings, while reflective of a player's overall performance, may not fully capture their ability to succeed to all surfaces. Clay courts, in particular, continue to challenge even the highest-ranked players, demonstrating the advantage of a diverse skill set in professional tennis.

## Conclusions and Future Research

This analysis questions the narrative that WTA players are more prone to upsets than their ATP counterparts. The data shows a statistically insignificant difference in average upset frequencies between the ATP and WTA tours, casting doubt on the idea that WTA players are inherently less consistent. The BO5 format has a significant effect on reducing upset frequency compared to the BO3 format. Despite this, we found no significant difference in upset frequency at the Grand Slam level, where the ATP uses the BO5 format, and the WTA uses the BO3 format.

Looking at historical trends, the WTA had a lower upset frequency than the ATP at the turn of the millennium. The period where the ATP consistently led the WTA in lower upset rates seems heavily influenced by the dominance of a few top players, creating a wide gap between the elite and the rest of the field. In contrast, the WTA had a more level playing field, where dominance was less concentrated. Additionally, differences in playing surfaces and match formats likely have a larger effect on upset frequency than whether the players are men or women.

I conclude that there is insufficient evidence to support the claim that WTA top seeds perform worse than ATP top seeds from 2000 to 2024. A fairer comparison would require introducing a BO5 format for WTA Grand Slams, which would eliminate this fundamental difference in match structure. Despite the current disparity, WTA top seeds often overcome these differences and show comparable performance to ATP top seeds. Ultimately, it’s inaccurate and unproductive to spread narratives about WTA players being more inconsistent without substantial evidence to back those claims.

This dataset also presents opportunities to explore predictive modeling for identifying factors that make top seeds more prone to upsets. This is something I would be very interested in pursuing in the future. Incorporating variables like player fatigue, injuries, and surface-specific performance could further enhance the analysis. As mentioned before, access to historical betting odds would also  enhance our analysis. I also plan to update this dataset at the end of each year’s tennis circuit to keep these insights relevant and expand on this work.
