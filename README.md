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



  
