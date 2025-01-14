# Professional Tennis Seed Performance Analysis (2000-2024)

### Project Overview

This data analysis project aims to identify trends in the performance of professional tennis seeds in the 21st century as well as to explore several factors that might have an effect on the frequency of “upsets”.

The inspiration for this project comes from my experiences growing up as a tennis player and an avid follower of professional tennis. The sentiment “the WTA [women’s] tour is inconsistent compared to the ATP [men’s] tour” is something I've heard often, especially after every surprise loss on the WTA tour. This is usually followed up discourse around the differences between women and men physically and emotionally, but I was more interested in exploring the validity of the base claim analytically while exploring some factors that influence this perception. 

An important goal of this project is to examine differences between ATP and WTA tour seed performances in order to begin to address claims about the inconsistency about the WTA, as well as to inform conversations around the topic of changing the WTA Grand Slam match format match the ATP tour's format, whcih is best-of-five sets rather than best-of-three sets.

### Important Links
   1. [Macro Script](./MacroScript)
   2. [Raw Data](https://docs.google.com/spreadsheets/d/1Q-vmwI6gNoF-C_xwDSoAolvSXmiG6Af1-6gGYoAU-7I/edit?usp=sharing)
   3. [Google Colaboratory File](https://colab.research.google.com/drive/1GCSkMTTcY_qCbacul9bb1TSKv7E0boAz?usp=sharing)
   4. [Expanded Data Table](https://docs.google.com/spreadsheets/d/1ADziKA-sanMPxB8ABH7TXf9HVaVAK3V8XUj0JIXK_uM/edit?usp=sharing)
   5. [Histogram Data Table](https://docs.google.com/spreadsheets/d/1OJNte-aFmfc5PyOiFCanwF82LV0MM0FprKp13VLqOy8/edit?usp=sharing)
   6. [Power BI Dashboard](https://app.powerbi.com/reportEmbed?reportId=ebd90410-c30f-43b7-8f37-77e73fdd126f&autoAuth=true&ctid=8b1f75d2-b3e9-4b3d-a9f2-0b5fac458a8d)

### Data Sources + Collection Method

The first step in collecting usable data for my purposes is to gather it from the Wikipedia pages for each tennis tournament. These pages list the seeds and how well they performed in that tournament, as well as several fundamental characteristics of the tournament including surface played on, tournament value, and size of draw. 

Originally, I was collecting the data by hand, but this proved to be tedious and unreliable. With the help of AI tools and Python HTML readers such as BeautifulSoup, I was able to automate this process. Since the formatting is often inconsistent between these Wikipedia pages, I then screened the results afterwards to ensure the data was complete and accurate. Afterwards, I created a variable which indicated if a player withdrew before the tournament, meaning they didn't play any matches and would not provide useful data. I wrote a [macro](./MacroScript) that allowed me to find each player’s *adjusted seed*, or their ranking against all of the players who actually played in the tournament, after excluding those who withdrew beforehand. The raw data that resulted from this process can be found [here](https://docs.google.com/spreadsheets/d/1Q-vmwI6gNoF-C_xwDSoAolvSXmiG6Af1-6gGYoAU-7I/edit?usp=sharing).

After collecting the raw data, I wanted to create a boolean that represents whether or not an observation was considered an “upset”. The first part of this was removing seeds which, based on my experiences following tennis, had expectations that did not meet the criteria for being susceptible to an upset. This is because even if a seeded player loses to an unseeded player, this wouldn't necessarily fit the standard idea of an upset. We call the seeds that remain from this process "upsettable seeds", but for the sake of clarity I will just refer to them as "seeds" in the analysis portion. Ideally, if I had the betting odds for each match I would be able to better define this, but using the data I had, I developed an acceptable metric for my purposes. We say that a player in a tournament was upset if they were more than 2 wins away from reaching their expected rounds won based on their adjusted seed value. All of the intermediate steps I am glossing over here are covered in full in the [Colaboratory](https://colab.research.google.com/drive/1GCSkMTTcY_qCbacul9bb1TSKv7E0boAz?usp=sharing). 

I also created a player appearances column which allows you to see how many data points there are for that particular player, resulting in the following [expanded data set](https://docs.google.com/spreadsheets/d/1ADziKA-sanMPxB8ABH7TXf9HVaVAK3V8XUj0JIXK_uM/edit?usp=sharing). In order to easily make a histogram in Power BI, I also created a [histogram table](https://docs.google.com/spreadsheets/d/1OJNte-aFmfc5PyOiFCanwF82LV0MM0FprKp13VLqOy8/edit?usp=sharing) specifically for that purpose, with only one line per player. Both of these spreadsheets can be downloaded upon compiling the Google Colaboratory file.

****Note on Tournament Values****

Determining whether a tournament’s value was Grand Slam, Olympics, or Finals was fairly straightforward. It is worth noting that determining whether a tournament was a 250, 500, or 1000 level required slightly more subjectivity. For this portion, I referenced both the approximate amount of points granted to the winner of the tournament and the relative frequency of each kind of these tournaments as they are clearly defined in recent years.

### Findings

I highly recommend exploring the [interactive Power BI dashboard](https://app.powerbi.com/reportEmbed?reportId=ebd90410-c30f-43b7-8f37-77e73fdd126f&autoAuth=true&ctid=8b1f75d2-b3e9-4b3d-a9f2-0b5fac458a8d) that I created and will be referring to for much of this section. The blue and orange colors represent the ATP and WTA tours respectively (note that the colors for the surface visuals represent the surface values). Furthermore, for all of the significance tests, I use the standard α = 0.05.

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


We run Welch’s t-test and arrive at the conclusion that the differences in average upset frequency between ATP and WTA is statistically insignificant (in both tables), so we fail to reject the hypothesis that the difference between the two is 0. Furthermore, we can observe from the histograms/tables that the variances for the ATP and WTA upset frequencies are approximately equal as well. In all, we observe virtually identical samples for both tours. This already helps us disprove the base claim, but there are some more interesting conclusions to be found when we break this down even further.

#### Match Format

<img width="437" alt="Image 3" src="https://github.com/user-attachments/assets/a3b9407b-1e6a-4360-a540-e491985e0eec" />

| Group  | n | Mean | Variance |
| ------------- | ------------- | ------------- | ------------- |
| Best of 3  | 12068  | 0.3501 | 0.2275 |
| Best of 5  | 1584  | 0.2235 | 0.1736 |

At a statistically significant level (p ≈ 3.3 × 10<sup>-28</sup>), the average upset frequencies between matches that are a best of 3 vs best of 5 set formats are different, with best of 3 set matches having approximately a 35% upset rate, almost 13% higher than the best of 5 set matches, which is at approximately 22%. To compare how the tours do in best of three set matches, we will look at the difference in upset frequency outside of the Grand Slams, which is where the only best of five set matches are played.

#### Tournament Value

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

#### Trends over Time



### Conclusions and future research

As a result of these results, I come to the conclusion that the WTA and ATP tours perform nearly identical. I believe the difference in format at the Grand Slam level leads to a difference in upset frequency that will become realized as more data is added. To address the underlying claim that I aim to assess, I do not have any data which signals that the WTA players are less consistent overall than the ATP players.

I plan to update this data at the end of each year’s tennis circuit.
