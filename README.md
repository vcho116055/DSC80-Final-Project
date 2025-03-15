# Analysis of the Impact of Creep Score Lead vs Kill Lead on Winning in League of Legends Pro Matches

by Vincent Cho (vicho@ucsd.edu)

---

## Introduction

In this project, I utilized Oracle's Elixir League of Legends pro match data from 2024. The question I aim to tackle is: does an early creep score (CS) lead matter more than an early kill lead? This is a pivotal question as while kills are a big deal in terms of resources, giving both EXP and gold while also depriving an enemy player from collecting either resource, CS is the main and most consistent source of income in the game. Therefore, both factors are highly contributive to the outcome of a game. 

There are 117576 rows and 161 columns, out of which I am only interested in the columns: gameid, result, csdiffat10, killsat10, and opp_killsat10. gameid gives a unique game identifier, allowing me to separate unique games. result gives either 0 for a loss or 1 for a win, which is necessary to hypothesis testing. csdiffat10 is quite self explanatory, it provides the CS difference between the two teams at 10 minutes, which is essential as my entire question revolves around this statistic. The same goes for killsat10 and opp_killsat10, each stat shows the amount of kills each team has at the 10 minute mark, which is necessary as my question also requires that I have access to this statistic. 

---

## Cleaning and EDA

In order to clean the data, I removed all rows in which the 'datacompleteness' column was marked as 'partial', as those rows were missing a significant amount of their data and would not be of much use. I then removed all rows in which the 'participantid' column was not set to 100, as other participantids than 100 and 200 applied to individual player stats. In the case of participantid 200, those rows referred to the other team, and thus, would mirror every datapoint I had and make it impossible to come to a conclusion. I then dropped every column that I did not need, giving me a dataframe with only these columns: gameid, result, csdiffat10, killsat10, and opp_killsat10. I also assigned a new column that processed the killsat10 and opp_killsat10 stats to calculate the kill difference at 10 minutes, named killdiffat10. Finally, I assigned a column called results_str that made it easy to hue my plots, which basically assigned either 'Game Won' if the result column had the value 1, or 'Game Lost' if the result column had the value 0. 

This plot is a histogram of the distribution of CS differences within the dataframe. There is a very obvious normal distribution in CS differences, centered at 0 CS difference. 

<iframe
  src="assets/fig.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

This plot is a scatterplot in which the x-axis represents the CS difference, the y-axis represents the kill difference, and the hue represents either a won or a lost game. There is a noticeable shift in wins and losses 

<iframe
  src="assets/fig0.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

This table displays the correlation between early CS leads, early kill leads, and results. 

|              |   csdiffat10 |   killdiffat10 |   result |
|:-------------|-------------:|---------------:|---------:|
| csdiffat10   |     1        |       0.206053 | 0.32504  |
| killdiffat10 |     0.206053 |       1        | 0.313346 |
| result       |     0.32504  |       0.313346 | 1        |

---

## Assessment of Missingness

The column 'url' is NMAR. Rows with a url value of nan either don't have stats or there are no streams or recordings of the game. The way that this could potentially be resolved into being MAR is by adding another column that contains a description of the url, i.e., whether it comes from a stream, a website, or doesn't exist. 

In this table, every p-value per column in my reduced dataset with the exception of result is 0. 

|    | Column        |   P-Value |
|---:|:--------------|----------:|
|  0 | year          |     0     |
|  1 | playoffs      |     0     |
|  2 | game          |     0     |
|  3 | patch         |     0     |
|  4 | result        |     0.146 |
|  5 | goldat10      |     0     |
|  6 | opp_goldat10  |     0     |
|  7 | csdiffat10    |     0     |
|  8 | killsat10     |     0     |
|  9 | opp_killsat10 |     0     |
| 10 | killdiffat10  |     0     |

---

## Hypothesis Testing

Null Hypothesis: CS difference and Kill difference at 10 minutes have the same effect on winning. 

Alternative Hypothesis: CS difference or Kill difference at 10 minutes has a significantly stronger effect on winning. 

Test Statistic: Mean Difference - Good choice as it is good at comparing the average effects of a feature

Significance Level: 0.05 - Standard significance level, one that I am used to using throughout my stats and probability classes. 

P-Value: 0.0

Based on the resulting p-value, the null hypothesis should be rejected. According to this test, there is sufficient evidence to say that one of either CS difference of kill difference at 10 minutes has a significantly stronger effect on winning. 

---