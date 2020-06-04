# Capstone Project : The Battle of Neighborhoods
Clustering Analysis of Toronto Neighborhoods

## 1.	Business Problem
This study is aimed to help a small vegan restaurant willing to open a new location in Toronto. The investors easily agreed to expand their activities in one of its neighborhoods as Toronto is the most populated city in Canada with high quality of living. 
Beyond this information, the investors seem to struggle in gathering sufficient details that will help them identify the appropriate location in Toronto where to establish their facility. Therefore, they requested an analysis of the Toronto market to drive their decision.

The suitable location is described as follow by the investors: 
* A neighborhood with an average to above average total population.
* A neighborhood with an average to above average net income.
* A neighborhood with an average to above average employment rate
* Above average populations of working individuals (between 25 and 45-year-old)
* A high concentration of the population having a higher education


The goal of this analysis is to identify and recommend to the investors which neighborhood(s) of Toronto will be the best choice to establish their facility. The target audience is vegan oriented businesses and restaurants/bars. Starting from these requirements and using data about neighborhoods population, activities and venues, the study will highlight most attractive opportunities.

## 2.	Data
Collecting relevant data is the first step to take in order to address this problem. Mainly, the data source identified are the City of Toronto and Foursquare API combined with additional data scrapped on Wikipedia.

#### Neighborhood Demographic
This dataset gathers information about age and sex, families and households, language, immigration and internal migration, ethnocultural diversity, Aboriginal peoples, housing, education, income, and labor. City of Toronto Neighborhood Profiles use this Census data to provide a portrait of the demographic, social and economic characteristics of the people and households in each neighborhood.

For administrative purposes, the City of Toronto divides the city into 140 neighborhoods and grouped into six distinct boroughs. The data is sourced from a number of Census tables released by Statistics Canada.<br>
[City of Toronto Open Data Source on Neighborhood Profiles](https://open.toronto.ca/dataset/neighbourhood-profiles/)

#### Neighborhoods Shapes
The City of Toronto provides also a GeoJSON data about boundaries of City of Toronto Neighborhoods that will be used for mapping purposes.<br>
[City of Toronto Open Data Source on Neighborhood Shapes](https://open.toronto.ca/dataset/neighbourhoods/)

#### Neighborhoods Boroughs
Additional information about boroughs provided by Wikipedia is acquired using web scrapping.<br>
[Toronto Neighborhood Borough Designation on Wikipidea](https://en.wikipedia.org/wiki/List_of_city-designated_neighbourhoods_in_Toronto)

#### Neighborhoods Venues 
Finally, data about venues in each neighborhood will be collected by making call to the Foursquare API. 

Using these datasets, the analysis will discover patterns in the 140 neighborhoods and carry out a segmentation of these neighborhoods through clustering techniques. Capturing these clusters will highlight areas in Toronto that holds similar features. The group of investors will then have suggested areas corresponding to their requirements and all the potential locations for their installation.

## 3.	Methodology
The analysis aims to suggest a target neighborhood to the client based on their requirements and the attibutes of each Toronto neighborhood. In order to achieve this goal, the study will explore demographic information about Toronto neighborhoods, clean and narrow down collected features using client requirements as a guide. Additional data, will be collected through web scrapping and API in order to gain more useful knowledge. The exploration will be mainly done using descriptive statisctics and folium maps for visualization. 

The main goal being the identification of potential locations for the establishing a new facility in Toronto, we will adress that problem using K-Means Clustering Algorithm. This choice is justified by the attempt to determine similar groups in an unsupervised envrionment. Since we don’t have the ground truth to compare the output of the clustering algorithm to the true labels in order to evaluate its performance, we only want to try to investigate the structure of the data by grouping the data points into distinct subgroups.

The clustering analysis is divided as follow:
    * Data Collection
    * Data Preprocessing and Cleaning
    * Data Exploration
        
### Step 1: Data Collection and Preprocessing

#### Dataset 1 - Demographic Features
1. Pulling demographic data from a CSV file provided by the City of Toronto.
2. Filtering the data using the client requirements as guide. Selected features are : population, net income, higher education, employment rate and number of adults in each neighborhoods.
3. The dataframe is presented as follow:

![](https://github.com/elliass/capstone_project/blob/master/images/neighborhood1.png)

#### Dataset 2 - Boroughs Names and Numbers
1. Scrapping data about borough names and numbers in order to align them to respective neighborhoods. The information is available on Wikipedia: Toronto Neighborhood Borough Designations.
2. Transforming and merging the data to previous dataframe.
3. The dataframe is presented as follow: neighborhood.

![](https://github.com/elliass/capstone_project/blob/master/images/neighborhood2.png)

#### Dataset 3 - Geographic Coordinates
1. Pulling geographic data from a GEOJSON file provided by the City of Toronto.
2. Filtering, transforming and merging the data to previous dataframe.
3. The final dataframe is presented as follow: neighborhood.

![](https://github.com/elliass/capstone_project/blob/master/images/neighborhood3.png)

### Step 2: Data Exploration Based on Venues
* Mapping each neighborhood using folium library.
* Calling the Foursquare API in order to collect the top 100 venue categories for each neighborhood in a radius of 1km.
    * Filtering data to get only the coordinates for each venue and the category it belongs to.
    * Visualizing all venues in a map.
    ![](https://github.com/elliass/capstone_project/blob/master/images/top100venues1.png)
    * Transforming data using one hot encoding. Then, grouping the venues by neighborhood using their mean value.
        * The dataframe is presented as follow:
        ![](https://github.com/elliass/capstone_project/blob/master/images/top100venues1.png)
    * Filtering and ranking venues the most common venues in each neighborhood.
* Clutering based on venues categories.
    * Tunning the parameter (k) using Elbow Method to get the optimal number of clusters.
    * Merging the cluster labels to main dataframe and mapping the clusters.

![](https://github.com/elliass/capstone_project/blob/master/images/cluster1.png)
    
    
### Step 3: Data Exploration Based on Requirements
* Discovering the top 10 neighborhoods based on client requirements: population, net income, education, adults and employment rate.
* Manually standardizing the data to give a positive value for neighborhoods above average and a negative value for the ones below the threshold.
    * Establishing the mean and standard deviation for each feature.
    * Calculating new values and replacing older values in previous main dataframe.
    * The dataframe is presented as follow: features.
* Clustering scaled features.
    * Tunning the parameter (k) using Elbow Method to get the optimal number of clusters.
    * Merging the cluster labels to main dataframe: checkpoint and mapping the clusters.

### Step 4: Data Exploration Based on Scoring System
* From the scaled values previously calculated, a scoring system is established.
    * Each attribute was standardized and then multiplied by a factor depending on the importance accorded to each attribute by the client.
    * Attributes are summed up afterwards in order to generate a score for each neighborhood.
    * The data is then merged to main dataframe and presented as follow: merged
* Filtering top 15 neighborhoods using the calculated score
* Repeating each point of step 2 and collecting venues only for those top 15 neighborhoods
    * Filtering, transforming, grouping and mapping venues
* Clutering based on venues categories
    * Tunning the parameter (k) using Elbow Method to get the optimal number of clusters.
    * Merging the cluster labels to main dataframe: filtered and mapping the clusters.
    
## 4. Results

    Insert based on venues map
    
Identifying groups in the data only based on venues does not produce sufficient information. Even the optimal number of clusters is complex to determine when considering the average venue categories for each neighborhood.
* What do these clusters represent ?
* How are they related or why are they similar ?
* How many clusters are present in this dataset ?

These questions demonstrate the need for gathering additional data. The next step, will try to highlight cluster inside the demographic data collected initially. As a first step, the neighborhoods were ranked with respect to the client requirements and the results identified 12 neighborhoods as performing better than other neighborhoods for more than one criteria. 
            
    Insert top_df 
            
Then, passing only the attributes as input and k=3 clusters, the algorithm successfully managed to find out a group of 13 neighborhoods (in lightgreen) that seems to confirm this previous ranking. In fact, most of them appeared in the above list.
    
    Insert based on requirement map
    
In order to extend the analysis, both datasets about venues and demographic were combined and filtered to the 15 most performing neighborhoods using a scoring sytem. Again, the 13 neighborhoods previously identified by the k-mean algorithm came back in the newly selected neighborhoods. From there, the venues around these locations were analysed in order to highlight other clusters within the 15 neighborhoods. 

    Insert last map
                
According to this last visualization, the top 15 neighborhoods chosen seems to be very much similar as it did not successfully split the neighborhood into clusters. Combining this result to previous analysis, we came to the concluison that these neighborhoods are the one that best meet the client requirements and tend to be similar based on the venue catagories.

## 5. Discussion
Based on the result given by this study, several recommendations can be made:
* The data highlighted 15 potential neighborhoods corresponding to the client requirements.
* The neighborhood of Waterfront Communities — The Island seems to be the best choice to go for. With the highest score among all neighborhoods, this area is a prime location for a restaurant.
* The neighborhoods of Annex, Church-Yonge Corridor, Dovercourt-Wallace Emerson-Junction, Niagara, are higly competitive. They record a total of 100 venues and can be easily explained as they are the closest neighborhoods to the city of Toronto.
* However, the neighborhoods of Willowdale East and Islington-City Centre West are ranking in the top 3 with respect to the overall score as well as havinf less competition (with only 57 and 58 venues respectively)
* The least options to consider seems to be the neighborhoods of Rouge and Malvern. They are the weakest performers in terms of score and have a poor number of venues as well. Additionally, their location is extremely isolated and would propably not be a good choice as the restaurant needs to be easily accessible.

## 6. Conclusion
In order to address the client business problem, the analysis needed data to be collected from various sources (files, web scrapping and API calls), cleaned and then merged. This goal was mainly achieved by exploring the dataset through descriptive statistics and visualization that generates useful insight to better understand Toronto neighborhoods. Finally, based on the data served to the k-means algorithm, the analysis successully highlighted the most attractive neighborhoods to recommend to the client with respect to their requirements.

Although the results seemed to be consistent throughout the study, some limitations should be disclosed. Gathering precise data is not always an easy task, especially when it comes to demographic information. The City of Toronto organise a census every five years about its population. As a matter of fact, the demographic data used to make those recommendations was actually collected by the City of Toronto in 2016. We can easily expect that the situation has changed since then and include that in the decision process. 

Areas of improvement should be specified as well for an upcoming analysis:
* Narrowing down the venue categories to the sub categories similar to the client project/industry to idenfiy more relevant neighborhoods inside the final cluster
* Collecting additionnal data to better understand the Toronto market
* Updating the analysis with the upcoming 2021 City of Toronto census
