# Blaise_Stats #
## My own sabermetrics website ##

### To view my web app go [here](http://www.blaisestats.com/) ###

If you prefer to run this application locally run the following commands:

```shell
git clone https://github.com/BlaisePage/Blaise_Stats
```
```shell
cd Blaise_Stats/src
```
```shell
export FLASK_APP=app.py
```
```shell
flask run
```

Note, that this code has a few package dependencies. In order to install the correct packages run:
```shell
pip3 install flask

pip3 install pandas

pip3 install bokeh

pip3 install numpy

pip3 install os
```
Once the app is running navigate to the corresponding localhost port to view the app.


## Report ##

I developed this web app for my CSCI 4831: Sabermetrics course in Spring of 2019. For this assignment, I was tasked with creating a new baseball statistic that evaluates players differently compared to existing statistics. To meet this criteria, I generated two new statistics that I dubbed Early Game Slugging (EGS) and Late Game Slugging (LGS). The purpose of these statistics are to analyze a player's batting productivity in late game scenarios compared their productivity in the beginning of the game. I have chosen to define "late game scenarios" as the 8th inning and above for any single game. Consequently, I define the "early game" as innings 1 through 7.

In order to calculate this statistic, I used the traditional Slugging Percentage equation, but I split the inputs two categories - late and early game situations. For reference, the Slugging Percentage equation is as follows:

SLG = (1B + 2B\*2 + 3B\*3 + HR\*4)/AB

where 1B=singles, 2B=doubles, 3B=triples, HR=home runs, and AB=at bats

Therefore:

LGS = (1B<sub>late game</sub> + 2B<sub>late game</sub>\*2 + 3B<sub>late game</sub>\*3 + HR<sub>late game</sub>\*4)/AB<sub>late game</sub>

EGS = (1B<sub>early game</sub> + 2B<sub>early game</sub>\*2 + 3B<sub>early game</sub>\*3 + HR<sub>early game</sub>\*4)/AB<sub>early game</sub>


Moreover, once I calculated a players EGS and LGS using the SLG equation above, I took the difference of the two metrics (LGS - EGS) to get a stat that I call SLG_GAIN. SLG_GAIN can be thought of as how much a given player's slugging percentage increases in late game scenarios compared to the previous innings. By fault, a positive SLG_GAIN means that a player is more productive at bat late in the game, while a negative SLG_GAIN means that the player's batting productivity declined in the later innings.

EGS, LGS, and SLG_GAIN are obviously closely related to the SLG metric; however, they also incorporate a time factor that is similar to correlating SLG to inning number. Moreover, EGS and LGS may be more insightful than a simple correlation between inning number and SLG because it also depicts a player's previous SLG during a given set of innings (correlations alone can not accomplish this). Another stat that can be related to EGS, LGS, and SLG_GAIN is clutch hitting. Although clutch hitting focuses more on the state of a given game compared to EGS, LGS, and SLG_GAIN, these metrics can still be used to evaluate whether or not a batter will be productive in late game scenarios.

I believe that the EGS and LGS statistics provide fans and managers with interesting and helpful data regarding players on both sides of the field. One example that comes to regarding the usefulness of this statistic is selecting relief pitchers in late game scenarios (innings 8 and 9). In this example, it may be worth it to check consult the LGS and SLG_GAIN for upcoming batters in the batting order. If the upcoming batters have a relatively low LGSs or a relatively large negative SLG_GAINs, it might be a good idea to play a lower level relief pitcher and give the starters a rest because previous data shows that the upcoming batters lose productivity in these innings. On the other hand, if the upcoming batters have high LGSs or large SLG_GAINs, it might not be a bad idea to use one of the starting pitchers during this part of the game.

In order to calculate each player's LGS and EGS, I had to find a dataset that described the result of a given player's at bat as well as the inning corresponding to that player's at bat. I determined that the best dataset for these requirements was the Statcast database because it listed the player at bat (in form mlbam id), the inning of that at bat, and the event that resulted from that player's at bat. Using these three attributes, I was able to calculate each player's EGS and LGS metrics. However, it is extremely important to mention that the technology used to create the Statcast datasets was not installed in every MLB stadium prior to 2015. In order to eliminate bias, I chose to only use the Statcast from the 2016, 2017, and 2018 regular seasons. Another notable limitation of the Statcast dataset is its size. Since my goal of this project was to create an interactive web app, I chose to query the Statcast datasets before had so that my app did not have to waste time querying the data each time a player was selected. Instead my web app simply looks up the requested players SLG, EGS, and LGS metrics in a previously calculated dataset that I created (for the full dataset that I created click [here](https://github.com/BlaisePage/Blaise_Stats/blob/master/src/statcast_data/final_slg_data.csv)). The final issue that I ran into was mapping mlbam IDs to player names. Due to the fact that mlbam IDs may not return a singular player name, I had no way of dynamically calculating a player's name based of their mlbam ID alone. In order to avoid conflict, I simply omitted players whose mlbam lookup generated more than one entry (for a full list of players omitted by this rule click [here](https://github.com/BlaisePage/Blaise_Stats/blob/master/src/helper_code/players_not_available.txt)).

In the end, it is evident that EGS, LGS, and SLG_GAIN generate interesting insights into how batting productivity is effected by the inning that a given player is at bat. As discussed previously, one can use the LGS and SLG_GAIN metrics to predict whether or not a player will have a impact at the plate come late game.
