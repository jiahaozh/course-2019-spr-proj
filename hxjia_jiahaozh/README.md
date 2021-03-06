
# CS504 Data-Mechanics Project

## Authors
Haoxuan Jia(hxjia@bu.edu)
Jiahao Zhang(jiahaozh@bu.edu)

## Framework and Programming Languages:
Flask, Python, HTML, CSS, Javascript

## Introduction
Since 2008, guests and hosts have used Airbnb to travel in a more unique, personalized way. Boston as a famous city for travel, the number of Airbnb houses has increased greatly these years, making Airbnb house choosing a time consuming thing to do.
<br />
The purpose of our project is to pick out houses with the best values for users and save their time of going through the whole list of Airbnb houses. We decided the best way as choosing houses with lower prices and higher review scores. Figure 1 and Figure 2 show the basic housing distribution scatter plot on Google map and the corresponding heatmap to show the housing density in areas. At first, for statistical analysis part, we used seaborn library to plot prices distribution and review scores distribution. And we calculated the correlation coefficient between prices and review scores and the the correlation coefficient between prices and the number of reviews. For optimization and constraint satisfaction part, we needed only those houses, the number of reviews of which is larger than 10 because small number of reviews is not representative. Then we calculated Within-Cluster-Sum-of-Squares(WCSS) under different K values to choose best K to do kmeans based on prices and review scores. Next we did kmeans to cluster houses.  Finally, we plotted each cluster according to latitudes and longtitudes on googlemap using different colors.
<img src="https://github.com/jiahaozh/course-2019-spr-proj/blob/master/hxjia_jiahaozh/Experimental_Results/Housing_Scatter_Map.png" />
<p align="center">Figure 1</p>
<img src="https://github.com/jiahaozh/course-2019-spr-proj/blob/master/hxjia_jiahaozh/Experimental_Results/Housing_Heap_Map.png" />
<p align="center">Figure 2</p>

## Authentication
We used Google Map API in KMEANS.py to plot googlemap and the api key is required.

## Datasets
### Boston Airbnb Calendar
A csv file containing the price on a cetain day between year 2016 and 2017
<br />http://datamechanics.io/data/hxjia_jiahaozh/Calendar.csv
### Boston Airbnb Listings
A csv file containing detailed information about a house
chttp://datamechanics.io/data/hxjia_jiahaozh/Listings.csv
### Boston Airbnb Reviews
A csv file containing reviews given by users to a specific house
<br />http://data.insideairbnb.com/united-states/ma/boston/2019-01-17/data/reviews.csv.gz
### Boston Landmarks
A csv file containing landmarks in different neighborhoods of Boston
<br />http://bostonopendata-boston.opendata.arcgis.com/datasets/7a7aca614ad740e99b060e0ee787a228_3.csv
### US Holidays
A csv file containing date and holidays from 1966 to 2011
<br />http://datamechanics.io/data/hxjia_jiahaozh/US_Holidays.csv

## Data Transformation
### Price_and_Comments
Generated from Boston Airbnb Listings and Boston Airbnb Reviews
<br />Boston Airbnb Listings: Project to get data in a form of (listing_id, price, review_score)
<br />Boston Airbnb Reviews: Project to get data in a form of (listing_id, comments);
                             Aggregate to get a combination of comments for each listing_id
<br />Combination: product + Project to get the comments and review score for each listing_id, in the form of (listing_id, price, comments, review_score)
### Price_on_Holidays
Generated from Boston Airbnb Calendar and US_Holidays
<br />Boston Airbnb Calendar: Select the data with valid price;
                        Project to get data in a form of (date, price);
                        Aggregate to get the mean price for each date
<br />US_Holidays: Select holidays in the range of Boston Airbnb Calendar's dates
<br />Combination: Product + Project to get the mean price for each date and whether the date is a holiday, in the form of (date, avg_price, holiday)
### Prices_Landmarks_Listings
Generated from Boston Airbnb Listings and Boston Landmarks
<br />Boston Airbnb Listing: Select, Project  Aggregate to get (neighbourhood, the number of houses in that neighbourhood), Select, Project and Aggregate to get (neighbourhood, the mean price of houses in that neighbourhood).
<br />Boston Landmarks: Select, Project and Aggregate to get (neighbourhood, the number of landmarks in that neighbourhood)
<br />Combiantion: project to get  (neighbourhood, the number of landmarks,  the number of houses, the mean prices of houses in that neighbourhood)
### id_month_price_score_lat_long
This part was implemented in id_month_price_score_lat_long.py
<br />Generated from Boston Airbnb Listings and Boston Airbnb Calendar 
<br />Boston Airbnb Calendar: Select, Project and Aggregate to get (id, mean price of all dates of the year) 
<br />Boston Airbnb Listing: Select, Project to get (id, review scores, the number of reviews, longitude, latitude) 
<br />Combination: Select, Project to get (id, mean price, review scores, the number of reviews, longitude, latitude)

## Trial mode
For trial mode, we used 100 Boston Airbnb Listings data, 36500 Boston Airbnb Calendar data and 500 Boston Airbnb Reviews data. Since Boston Landmarks and US Holidays are quite small, we used all the data of these two dataset.

## Statistical Analysis
This part was implemented in KMEANS.py
<img src="https://github.com/jiahaozh/course-2019-spr-proj/blob/master/hxjia_jiahaozh/Experimental_Results/Price_Distribution.png" />
<p align="center">Figure 3</p>
<img src="https://github.com/jiahaozh/course-2019-spr-proj/blob/master/hxjia_jiahaozh/Experimental_Results/Review_Score_Distribution.png" />
<p align="center">Figure 4</p>
<br />We calculated the correlation coefficient between prices and review scores and the correlation coefficient between prices and the numbers of reviews. The results are as follows.
<br />Prices and Review Scores: 0.1172
<br />Prices and the numbers of Reviews: -0.1565 
<br />In the project we used price and review score to do kmeans in the next step.

## Methodology and Results
This part was also implemented in KMEANS.py
<img src="https://github.com/jiahaozh/course-2019-spr-proj/blob/master/hxjia_jiahaozh/Experimental_Results/K_WCSS.png" />
<p align="center">Figure 5</p>
The x-axis of Figure 5 is the range of k we ran.
<br />The y-axis within-cluster sum of squares is the sum of the squared deviations between each observation and the cluster center.
<br />The method we used to get the best value of k is the elbow method. This method plots a line chart of WCSS for each value of k. The "elbow" which is the most significant change point of the line chart, is the best k.
<br />We calculated the Within-Cluster-Sum-of-Squares(WCSS) for k-means and found the best k value is 4, so we did a 4-means and plotted each cluster onto googlemap to reveal the relationship between each cluster and their geolocation.
<img src="https://github.com/jiahaozh/course-2019-spr-proj/blob/master/hxjia_jiahaozh/Experimental_Results/Kmeans_Result.png" />
<p align="center">Figure 6</p>
This is the Kmeans result we got using the best k value(4). In the figure we can see that the houses are divided into four clusters and the cluster with the best value of money and review scores is the one in red.

<img src="https://github.com/jiahaozh/course-2019-spr-proj/blob/master/hxjia_jiahaozh/Experimental_Results/Classified_Map.png" />
<p align="center">Figure 7</p>
From Figure 6 we can see that the houses are basicly divided by price. <br />
In Figure 7 the houses with the highest recommendation are in red, most of which are distributed in Allston, East Boston and Mission Hill area.<br />
The houses with the second highest recommendation are yellow, most of which are distributed in Back Bay and Downtown area.<br />
The houses with the second least recommendation are in blue, most of which are mostly distributed in north-east corner of Boston.<br />
The houses with the least recommendation are in green. They are rare and have no specific distribution pattern.

## Improvements
We did 2 improvements on K-Means.
### 1. K-Means++
Although given enough time KMeans will always converge, this may lead to a local minimum. It is highly dependent on the 
initialization of the centoids. To overcome this issue, we used k-means++ initialization scheme. Just set init = 'k-
means++' parameter in Sklearn. This helped initializing the centroids to be (generally) distant from each other. And it helped 
speed up converging.

### 2. Mini Batch K-Means
The Mini Batch K-Means is a variant of K-Meanns, which uses mini-batches to shorten computation time. In contrast to other algorithms that reduce the convergence time of k-Means, mini-batch k-Means produces results that are generally only slightly worse than the standard algorithm.
As a result, the computation was shortened to its half while the WCSSs differed slightly.


## Web Visualization
We made three webpages.
### 1. Register/Login Page
<img src="https://github.com/jiahaozh/course-2019-spr-proj/blob/master/hxjia_jiahaozh/web_visualization/Login_page.png" /><p align="center">Figure 8</p>
### 2. User Parameter Input Page
* Users can select price ranges: $ 0 ~ 100, $ 100 ~ 200, $ 200 ~ 300, $ 300+ <br />
* Users can select review score ranges: ! 85 -, !! 85 - 90, !!! 90 - 95, !!!! 95 - 100
<img src="https://github.com/jiahaozh/course-2019-spr-proj/blob/master/hxjia_jiahaozh/web_visualization/Search_page.png" /><p align="center">Figure 9</p>
### 3. Recommendation Results Page
* Marked in map using Leaflet
* Urls listed as hyperlink
<img src="https://github.com/jiahaozh/course-2019-spr-proj/blob/master/hxjia_jiahaozh/web_visualization/Recommendation_page.png" /><p align="center">Figure 10</p>

## Future work
For features, we considered only prices and review scores in the projected. We will research into previous customers’ comments and used modern NLP technique to pick out highly rated houses more accurately.
<br />For algorithm, K-Means is a naive classification algorithm. We can use more complicated classification methods.
<br />For web application, it is not very user-friendly right now. We can add some interesting function like multiple login ways, search lists, add friends, etc.


# Tools and Libraries
<br />Pandas
<br />dml
<br />prov
<br />protoql
<br />json
<br />uuid
<br />urllib.request
<br />scipy
<br />sklearn
<br />gmplot
<br />matplotlib
<br />seaborn
<br />folium
<br />Leaflet

### If you have any problems, please feel free to contact us.
