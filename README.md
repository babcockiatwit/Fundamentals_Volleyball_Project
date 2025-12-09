# Observing and Modeling Change in Serving Metrics Over Time

# Introduction 
  This project was undertaken to observe the change of a variety of serving metrics over the course of a match. Some sought after questions were how does serve efficiency,percentage and errors acccumulate and change over the course of a match? Is there a way we can accurately model and forecast serve percentage in future case using ARIMA modeling? Lastly, how does the efficiency of a serve change based upon the type of serve. 

  This project was also undertaken due to the personal experiences with volleyball serving's significance and a desire to dive deeper into some notable metrics to gain greater insight and understanding to a sport that I was very familiar with. 

# Selection Of Data 

  My dataset was found via a google search of some datasets and studies related to volleyball. I quite frankly struck gold with a dataset related to a report from a group of students at the University of California Santa Barbara and can be found here: https://github.com/haotianxia/VREN/blob/main/dataset_full.csv . 
  The dataset includes 9 different matches spanning across NCAA division 1 and professional men's volleyball matches and contains all the rally information including location of hitter,passer and landing location, as well as block touch, rally outcome and many, many more. Below is the information related to our set. 
<img width="527" height="498" alt="image" src="https://github.com/user-attachments/assets/ffe9427d-40dd-4421-abbf-374cef2a6e69" />

  So as seen above we have a large number of null and nan values in our dataset. This is due to the not every value being needed for each 'round' of a rally. There won't be a win reason or lose reason if the rally is still ongoing. So for those values we set them to 'ongoing'. This is also true of our serve type, if there wasn't a serve type for that round it was set to 'continue'. also there was a pass rating system set that is fundamental to understanding efficiency and what qualifies as a 'good serve'. If the server get an ace (scores on that serve) the pass rating will be a zero. If the pass is in play but is far from the net it would get a rating of 1. If it is close to the net but pushed too far to the left or right that pass would receive a 2. If it's close to net and in the middle it would receive a 3. Our pass land location was essential for this. The dataset creators split the court into a 5 by 3 grid corresponding with a number and the surrounding area outside the court as well. This made our pass rating much easier to quantify using the below grid. (provided via the dataset).So for instance any pass in grid 13 would be considered a 3 pass. 
<img width="957" height="480" alt="image" src="https://github.com/user-attachments/assets/6dfe9e01-9304-4d2e-91c0-572d668de357" />


  Below is a more in depth look at the process of labeling each pass. 
<img width="1139" height="231" alt="image" src="https://github.com/user-attachments/assets/4318bbc5-8cb8-454d-8472-b02f9d8530a3" />

  Now since this data contains multiple matches, and comparing across matches would not be very feasible for measuring change over time, each match was then given a label for which match it was. This was found using the first rally label  of 1 and manually finding their respective indexes. Now that our matches have been found, We need to separate into serve only data which can be achieved by looking at the first round only ( start of a rally). After that it's time to start calculating some serve specific metrics. The first metric we want to look at is serve rating. Rating is on a 4 point scale, with 4 being an ace and 0 being an error. The in between metrics compare inversely to the pass rating associated with the serve. if the pass is a 3, our serve was 1, if the pass is a 2, our serve is a 2 and so on. Now the set must be split into its two teams respectively, to calculate the averages for each team (and later modeling makes much more sense to be used by team). By team, we calculate the running totals of serve attempts, aces, and errors per match , use these to calculate efficiency (aces-errors/attempts) , percentage ( attempts-errors/attempts) and average rating per match. That gives us our below final dataset.* 
<img width="603" height="731" alt="image" src="https://github.com/user-attachments/assets/c982bd2b-df69-4431-a9ff-fdf9ec2598bf" />
*just for team a 

  For the third question it was similarly divied up to how the team datasets were divided up but instead of by team we divided them by serve type and recalculated all the necessary metrics for our serving as demonstrated above. 
# Methods 
## Tools 
Pandas, Numpy, StatsModel (acf,adf,pacf, arima) , pmdarima (auto_arima) 
## Software ##
Jupyter Notebook
## Visuals ##
Seaborn, Matplotlib

# Results 
## Q1 How Do Serve Metrics Change Over Time 
  To observe this a handful of metrics were observed to observe the changes by team over the course of a match. For starters, The running count of service errors over the course of a match was observed. With the graph below we see that toward the beginning of a match we see more errors at the beginning of a match, which evens out in the middle of the match and then shoots off toward the tail end of the match. 
<img width="553" height="444" alt="download" src="https://github.com/user-attachments/assets/f9a2bf6b-c0a4-447c-8ee7-33a73f0a84e0" />
<img width="553" height="444" alt="download" src="https://github.com/user-attachments/assets/6217cade-211a-4a4a-ba2c-a0741831e427" />

  Then aimed to measure serve efficiency over time which we use a boxplot by team over the attempts. As shown below we see once again the extreme volatility at the beginning of the match and then suprisingly enough we see the majority of our outliers sit firmly in the middle of our dataset again as the data evens out before dropping out again toward the end of a match. 
<img width="573" height="444" alt="download" src="https://github.com/user-attachments/assets/60aed02b-f31b-4b42-9022-51540613d16a" />
<img width="573" height="444" alt="download" src="https://github.com/user-attachments/assets/e3744f6c-3f86-4bf1-b7f6-6380629d15ec" />

## Q2 Can We Model and Forecast Serve Percentage using ARIMA modeling? 

  The results of our next question in short is yes but it's a little bit deeper than that. We first needed to graph and understand each match for each team using the augmented dickey Fuller test to assure stationarity. *
* results for team a 
<img width="1006" height="209" alt="image" src="https://github.com/user-attachments/assets/256c0c0c-dd0e-4260-bdf2-c376b4fde1e7" />
  So for the most part our data was stationary but there was a variety of different match lengths and some teams in matches didn't even get to 60 serves total in a match. With some sample sizes such as this, an augmented Dickey Fuller test wouldn't be extremely effective. The stationarity of this dataset won't be too large of an issue with our matches with greater sample size which were stationary. Another way we can figure out the ordering of our ARIMA model is using the pacf and acf. Below is the best example of what a good stationary model would look like for an ARIMA(2,0,0) model. ( looking back on the previous 2 days results). We see a steady exponential decay of our autocorrelation function which is a key correlator of using AR lags. In our pacf we see 2 large spikes of correlation at the beginning with the rest of our values being "insignificant". Due to this we say that we are AR(2) because the 2 significant spikes represent significant lags. 
<img width="1222" height="288" alt="download" src="https://github.com/user-attachments/assets/e5010eda-30cc-4e08-9258-61b60b4b9e76" />
  Now due to the volatility,  I only fit our 3 largest serve samples per match (matches 1,3,5) to the arima model using auto_arima to reduce error for each team. Below is a graph of our actual vs our model for match 9 team b.
<img width="828" height="553" alt="image" src="https://github.com/user-attachments/assets/7bd4995e-b3d3-4c5d-9940-13fd5b894fc6" />

  Afterwards Some residual test took place using mean squared error we found that team b match 1 and 9 were our best results. We then strove to test the residuals of these two models farther. First by taking the acf, and pacf and a qq-norm plot of both residuals. Below are the results for match 1b. We find that our residuals are independent but they are not normally distributed. 
<img width="1222" height="288" alt="download" src="https://github.com/user-attachments/assets/164fe0da-516d-40c8-a23d-3b2d254f7a0c" />

<img width="562" height="444" alt="download" src="https://github.com/user-attachments/assets/2b77b738-d4cc-4645-82bb-ad2ed96ff2e4" />

  Using these model diagnostics and the forecast method built into auto_arima we predicted the percentage after the next 10 serves and here's what we got for match 1b. 

<img width="554" height="444" alt="download" src="https://github.com/user-attachments/assets/06edf50b-b01d-40cf-bde1-1a6a8a4dd370" />

## Q3 How does Serve Efficiency Change Depending on the Type of Serve?

  With our serve type split dataset we first took a line plot over time of the service errors over the course of a match for both teams. Which yielded that float serves are generally missed far less often than jump serves. There are also much less float serves in NCAA D1 and professional volleyball than jump serves. 

<img width="557" height="444" alt="download" src="https://github.com/user-attachments/assets/67c3cfe5-36a6-4296-8fcf-89ed7b66a1e3" />

<img width="553" height="444" alt="download" src="https://github.com/user-attachments/assets/5a3aa1c2-013d-42c8-a9c6-25925b794986" />

  Now taking a glance at efficiency,  jump serves are far more variant than jump serves with values stretching from perfect efficiency to negative efficiency of -.5 or -.6. Where as float serves tend to be consistently stable at zero. 

<img width="561" height="444" alt="download" src="https://github.com/user-attachments/assets/a207e048-90a3-4a83-b405-72fb869a8143" />

<img width="561" height="444" alt="download" src="https://github.com/user-attachments/assets/2d44a055-d48d-46b1-8d74-1fa1931dd45b" />

# Discussion

  For our first question, we see a consistent swing of invariance and unpredicitability at the beginning of each match, a rise and settling in the middle for each match and then a significant drop off toward the end of a match. These results note to me the affect of fatigue and warmups. At the beginning of a match, it takes acclimation to ease yourself into the game and to fully warmup. So we see large spurs of changes. As the game goes on we see a settling effect, as everything turns to the average and the middle with a few outliers.We see less errors, more efficient serves and better quality play. At the end of the game, fatigue starts to set in and that can be chalked up to the drop off we see at the back half of longer matches the overall decreased serving quality. This makes the most sense as the end of some matches you can feel that you're moving through molasses at times. This is important because teams can prepare for their serving to be less of an assett when the end of match begins to come around and can use subs for players whose serving has struggled the most as matches go on. 

  For our second question, the short answer is yes we are able to model serve percentage with ARIMA forecasting. Which in it's best case, we look back on the past 2 or even 4 results and account them into the model's next value. A lot of our data was not stationary for a couple of reasons, one being the small sample size of data and another being the sheer volatility that comes at the beginning of a metric like percentage. This can be greatly attributed to when we have less attempts our percentage will change more drastically before stabilizing out as the match goes on. Our residuals were also not normally distributed for our best cases but they were independent which does mean we can consider our best models valid. This matters because looking back on a teams previous two results could potential give you greater insight into how your gameplan surrounds the aspect of a teams serving. If they have been dipping down in serve percentage and that number continues to fall, that could be used a factor to shift momentum and take advantage of weaker periods for serving by your opponent. 

  For our third question, we can say that jump serving is considered far more high risk high reward as when jump serving is effective it is seriously punishing for an opposing team. Float serves are far more safe and errors with float serves are much less frequent but they come with downside of being far less efficient. 




  
