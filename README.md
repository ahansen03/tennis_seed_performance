# Professional Tennis Seed Upset Analysis (2000-2024)

## Background and Project Overview

In professional tennis, an "upset" is when a player that is expected to defeat their opponent easily ends up losing to them. A few notable examples of this are Roger Federer's loss to Sergiy Stakhovsky in Wimbledon 2013, Martina Navratilova's loss to Kathleen Horvath in the French Open 1983, and Rafael Nadal's loss to Robin Soderling in the French Open 2009. Every tournament one can expect to see at least one upset, if not a few. 

All of the examples of upsets that I just mentioned occurred during a Grand Slam, which is the highest value of professional tournament (2000 points granted to the winner's ranking). Other tournament values include 250s, 500s, and 1000s, whose value corresponds to the amount of points the winner receives. The Olympic Games, which do not currently grant points to the winner, are played every four years. There are also the "Finals", where the top ranked player at the end of the year play a tournament amongst themselves for upwards of 1500 ranking points. Typically, the Grand Slam and 1000 level tournaments have every healthy player trying to play them, whereas the others have a weaker entry list (fewer highly ranked players play them). The top 8/16/32 players (depending on the tournament value) in a tournament by ranking are called "seeds", and are usually granted a favorable draw, meaning they are positioned in the draw (or bracket) away from the other seeds and would not have to face another seed until the later rounds of the tournament.

The professional tennis circuit is divided into two "tours": the WTA (or women's) tour and the ATP (or men's) tour. The main difference between the tours in terms of format is that the ATP tour plays best of five sets for all matches in a Grand Slam, whereas every other match is a best of three sets format on both tours. This means that in a Grand Slam on the ATP tour, a player has to win three sets to win a match rather than the standard two. Otherwise, the two tours play many of the same tournaments, with the same tournament values, and a very similar schedule.

As someone who spends a lot of time following professional tennis year-round, I often hear the sentiment that the players on the WTA tour are less consistent than the players on the ATP tour. This rhetoric is frequently heard after every surprise exit of a top ranked WTA player in a tournament. This is often followed up with discourse around the differences between women and men physically and emotionally, which can be counterproductive and unfair. 

I aimed to analyze whether the WTA is more inconsistent than the ATP through statistical evidence. I will do this using supporting dashboard visuals and statistical analysis to compare upset frequencies and the variance in upsets. On top of this, I wanted to explore how the difference in match format may be more of a contributor than the physical and emotional differences between the players. Finally, as a tennis fan, I found it would be interesting to touch on how the different surface types (carpet, clay, grass, and hard courts) relate to the frequency of upsets, as the surface type affects the speed and bounce of the ball, potentially favoring certain playstyles and increasing the likelihood of upsets.

Furthermore, I believe my findings here are relevant to the discussion of whether the WTA and ATP should adopt the same Grand Slam format. This is a belief that has the support of many, most notably the founder of the WTA, Billie Jean King. King stated, "I think the women should play 3 out of 5 sets or everyone should play 2 out of 3. But if they play 3 out of 5 sets, then that means they have more content", highlighting how five-set matches create dramatic moments that captivate viewers. Moreover, it would be unfair for the top ranked men to outperform the top ranked women and gain more prize money simply because of the difference in match formats. If my analysis demonstrates this to be the case, that could help justify the need to standardize the Grand Slam match formats.

## Important Links
   1. [Macro Script](./MacroScript)
   2. [Raw Data](https://docs.google.com/spreadsheets/d/1Q-vmwI6gNoF-C_xwDSoAolvSXmiG6Af1-6gGYoAU-7I/edit?usp=sharing)
   3. [Google Colaboratory File](https://colab.research.google.com/drive/1GCSkMTTcY_qCbacul9bb1TSKv7E0boAz?usp=sharing)
   4. [Expanded Data Table](https://docs.google.com/spreadsheets/d/1ADziKA-sanMPxB8ABH7TXf9HVaVAK3V8XUj0JIXK_uM/edit?usp=sharing)
   5. [Histogram Data Table](https://docs.google.com/spreadsheets/d/1OJNte-aFmfc5PyOiFCanwF82LV0MM0FprKp13VLqOy8/edit?usp=sharing)
   6. [Power BI Dashboard](https://app.powerbi.com/reportEmbed?reportId=ebd90410-c30f-43b7-8f37-77e73fdd126f&autoAuth=true&ctid=8b1f75d2-b3e9-4b3d-a9f2-0b5fac458a8d)

## Data Sources + Collection Method

The first step in collecting usable data for my purposes is to gather it from the Wikipedia pages for each tennis tournament. These pages list the seeds and how well they performed in that tournament, as well as several fundamental characteristics of the tournament including surface played on, tournament value, and size of draw. 

Originally, I was collecting the data by hand, but this proved to be tedious and unreliable. With the help of AI tools and Python HTML readers such as BeautifulSoup, I was able to automate this process. Since the formatting is often inconsistent between these Wikipedia pages, I then screened the results afterwards to ensure the data was complete and accurate. Afterwards, I created a variable which indicated if a player withdrew before the tournament, meaning they didn't play any matches and would not provide useful data. I wrote a [macro](./MacroScript) that allowed me to find each player’s *adjusted seed*, or their ranking against all of the players who actually played in the tournament, after excluding those who withdrew beforehand. The raw data that resulted from this process can be found [here](https://docs.google.com/spreadsheets/d/1Q-vmwI6gNoF-C_xwDSoAolvSXmiG6Af1-6gGYoAU-7I/edit?usp=sharing).

After collecting the raw data, I wanted to create a boolean that represents whether or not an observation was considered an “upset”. The first part of this was removing seeds which, based on my experiences following tennis, had expectations that did not meet the criteria for being susceptible to an upset. This is because even if a seeded player loses to an unseeded player, this wouldn't necessarily fit the standard idea of an upset. We call the seeds that remain from this process "upsettable seeds", but for the sake of clarity I will just refer to them as "seeds" in the analysis portion. Ideally, if I had the betting odds for each match I would be able to better define this, but using the data I had, I developed an acceptable metric for my purposes. We say that a player in a tournament was upset if they were more than 2 wins away from reaching their expected rounds won based on their adjusted seed value. All of the intermediate steps I am glossing over here are covered in full in the [Colaboratory](https://colab.research.google.com/drive/1GCSkMTTcY_qCbacul9bb1TSKv7E0boAz?usp=sharing). 

I also created a player appearances column which allows you to see how many data points there are for that particular player, resulting in the following [expanded data set](https://docs.google.com/spreadsheets/d/1ADziKA-sanMPxB8ABH7TXf9HVaVAK3V8XUj0JIXK_uM/edit?usp=sharing). In order to easily make a histogram in Power BI, I also created a [histogram table](https://docs.google.com/spreadsheets/d/1OJNte-aFmfc5PyOiFCanwF82LV0MM0FprKp13VLqOy8/edit?usp=sharing) specifically for that purpose, with only one line per player. Both of these spreadsheets can be downloaded upon compiling the Google Colaboratory file.

### Note on Tournament Values

Determining whether a tournament’s value was Grand Slam, Olympics, or Finals was fairly straightforward. It is worth noting that determining whether a tournament was a 250, 500, or 1000 level required slightly more subjectivity. For this portion, I referenced both the approximate amount of points granted to the winner of the tournament and the relative frequency of each kind of these tournaments as they are clearly defined in recent years.

## Findings and Analysis

I highly recommend exploring the [interactive Power BI dashboard](https://app.powerbi.com/reportEmbed?reportId=ebd90410-c30f-43b7-8f37-77e73fdd126f&autoAuth=true&ctid=8b1f75d2-b3e9-4b3d-a9f2-0b5fac458a8d) that I created and will be referring to for much of this section. There are additional visuals and filters on there that I consider interesting for exploration. The blue and orange colors represent the ATP and WTA tours respectively (note that the colors for the surface visuals represent the surface values). Furthermore, for all of the significance tests, I use the standard α = 0.05 for Welch's t-test.

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


The difference in average upset frequency between the ATP and WTA tours generally and for the players with 10+ appearances is statistically insignificant. As a result, we cannot confidnetly make the claim that the two tours perform differently. Furthermore, we can observe from the histograms/tables that the upset frequency variances are approximately equal for both tours. This is all to say that we observe virtually identical distributions for both tours. This already gives us evidence against the base claim, but there are some more interesting conclusions to be found when we break this down even further.

### Match Format


<img width="437" alt="Image 3" src="https://github.com/user-attachments/assets/a3b9407b-1e6a-4360-a540-e491985e0eec" />


| Group  | n | Mean | Variance |
| ------------- | ------------- | ------------- | ------------- |
| Best of 3  | 12068  | 0.3501 | 0.2275 |
| Best of 5  | 1584  | 0.2235 | 0.1736 |


At a statistically significant level (p ≈ 3.3 × 10<sup>-28</sup>), the average upset frequencies between matches that are a best of 3 vs best of 5 set formats are different, with best of 3 set matches having approximately a 35% upset rate, almost 13% higher than the best of 5 set matches, which is at approximately 22%. To compare how the tours do in best of three set matches, we will look at the difference in upset frequency outside of the Grand Slams, which is where the only best of five set matches are played.

### Tournament Value


<img width="427" alt="Image 2" src="https://github.com/user-attachments/assets/9ccaf3f2-e188-48a5-b22c-533b1f28c48a" />


If we look at only the non-Grand Slam data, we observe the following:


| Group  | n | Mean | Variance |
| ------------- | ------------- | ------------- | ------------- |
| WTA  | 5026  | 0.3613 | 0.2308 |
| ATP  | 5458  | 0.3684 | 0.2327 |


The difference between the means is insignificant (p ≈ 0.45), hinting at the fact that outside of Grand Slams where both tours play the same best of three set format, the upsettable seeds in both tours tend to do relatively equally in terms of upset frequency.

At the Grand Slam level, we observe the following:


| Group  | n | Mean | Variance |
| ------------- | ------------- | ------------- | ------------- |
| WTA  | 1584  | 0.2513 | 0.1882 |
| ATP  | 1584  | 0.2235 | 0.1736 |


Although the means are almost 3% different, the difference between the means is insignificant (p = 0.06). With our current data we are unable to make the assertion that the seed in both tours perform differently at the Grand Slam level, despite the fact that the WTA play the best of 3 set format and the ATP plays the best of 5 set format, which intuitively would make upsets more common, as a lower ranked player only has to win 2 sets rather than 3 in order to beat the higher ranked player.

### Trends over Time


<img width="711" alt="Image 5" src="https://github.com/user-attachments/assets/0cbe60de-d5bf-4022-a41f-72b3ae653188" />


This image visualizes the average upset frequencies each year for both tours. From 2000-2008, we observe that the ATP's upset rate was consistently higher than the WTA's. Over this period, the ATP's average upset frequency was approximately 36.86% compared to the WTA's 26.93%, a difference of almost 10 points. After this period, it seems that the WTA's upset frequency has been higher than the ATP on average (37.04% vs 31.17% from 2009-2024). The reason for this shift is unclear, but it could have to do with the emergence of the "Big 3" on the ATP tour: Novak Djokovic, Roger Federer, and Rafael Nadal. These three were consistently dominant starting around the time of the shift, winning 38 out of 47 consecutive Grand Slams from 2009 to 2020. Andy Murray, another dominant ATP player during this time, won three grand slams and is often grouped with the Big 3 to form the "Big 4".

To put it into perspective, if we consider the time period 2009-2020 and only the 1000 and Grand Slam level tournaments, we observe the following:

\# of Unique Top 3 Seeds:
   - ATP: 13
   - WTA: 22

\# of Unique Top 2 Seeds:
   - ATP: 6
   - WTA: 19

\# of Unique 1 Seeds:
   - ATP: 4
   - WTA: 11

From this, it is clear that the very top seeds for the ATP were unusually dominant during this period. It is my belief that both tours go through periods of strength and weakness, which may end up becoming clear as we continue to collect more data. 

Furthermore, the data above does not tell the whole story. We already have seen that best of three set matches attract more upsets, and the highest valued tournament (Grand Slam) is played using best of three for WTA and best of five for ATP. Therefore, the entirety of the WTA ranking points comes from best of three set matches whereas a significant chunk of the ATP ranking points come from the best of five set matches, meaning the rankings for the ATP should be more representative of where the player actually stands amongst the crowd. Since we observe that the overall upset frequenecy is approximately the same for both tours despite this fact, this actually points in favor of the WTA's ability to overcome the limiting format to perform.

### Match Surface

It is well known amongst the tennis community that the surface that a match is played on can have a large effect on players. While a fast surface like grass courts favor large servers and big hitters, a slower surface like clay favors fast movers who can slide around on the surface and get to the ball quicker. It is important to notice that hard courts are the most common surface for a pro tournament to be held on (image 1) and so therefore a player's ranking may be more indicative of their ability to play on hard courts over anything else. Regardless, image 2 demonstrates the upset frequencies on each of the four surfaces (note that carpet surfaces are not used in any professional tennis tournaments as of 2024).

<img width="221" alt="Image 6" src="https://github.com/user-attachments/assets/55f8169a-5c8c-4c1e-a945-d0cfe00feb62" />  

<img width="271" alt="Image 4" src="https://github.com/user-attachments/assets/540e1836-8047-4484-81b0-3cd842e6dc28" />  

It is clear that upsets occur most frequently on carpet courts. With the current possibilities for court surface in the professional circuit, clay courts have the greatest upset frequency, at 36%, compared to grass courts at 34% and then hard courts at 32%. The difference in means between clay and hard courts is statistically significant (p ≈ 0.0003).

## Conclusions and future research

As a result of these results, I assert that there is insufficient evidence that the top seeds on the WTA tour perform worse than the ATP tours' top seeds from 2000-2024. Our analysis would be greatly enhanced by the addition of best of five set format for the Grand Slams on the WTA tour so we can have a fairer comparison. This fundamental difference limits our ability to compare the two accurately. Whether or not the WTA does adopt this best of five set format, I believe it is inaccurate and often unproductive to spread this narrative about the WTA players without proper research or basis of fact to back up the claim.

I am confident that this data can be used to train a model to predict the performance of upsettable seeds. This is something I would definitely be interested in exploring in the future.

I plan to update this data at the end of each year’s tennis circuit.
