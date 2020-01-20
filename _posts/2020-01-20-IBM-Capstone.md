# The Battle of Neighborhoods(Week1)
**Meeenakshi Dubey**

## <span style="color: steelblue;"> Where to live in Canberra?</span>

 

### <span style="color: slategrey;"> Introduction </span>

Chossing a place to live is a common and stressful problem faced by individuals when they move. The problem becomes a nightmare when you have to move to a city like Canberra, whose house rental prices are increasing every minute[1][2]. Moreover, the situation is worsened by the poor transport system in Canberra[3] coupled with its explosive growth[4]. 

In this dire situation it becomes even more important and stressful situation for an individual to objectively decide the suburb or area to settle in Canberra. Hence to tackle this problem we are developing a tool to aid individuals in making an informed choice as per the personal preferences based on the location data given by the FourSquare API[5] and rent data by SQM Research[6]. We look to find out the place most suitable for an individual based on preferences about a location and the expected renting cost. 

The case presented in this project is a very frequently occuring phenomenon for many working individuals and their respective workplaces. Hence the project is of great interest to them to sort out the relocation situations amicably and in an informed manner. For this project, we are assuming a married couple has moved to Canberra with their 14 year old daughter. The couple needs to figure out a place that gives resonable commute to their workplace along with a location that is suitable for development of their daughter. The project focuses on a specific case by assuming preferences of the couple, however the project can be easily adapted to suit various different situations  encountered by changing the prefferences about the location for a different usecase.

### <span style="color: slategrey;"> Data </span>

For this project we will use, four different sources of data:-
1. Suburbs of Canberra with their postcodes[7]
2. GeoCoder API extracted geo locations of suburbs in Canberra[8]
3. FourSquare API extracted venues by Categories for suburbs in Canberra[5]
4. Canberra house rental prices by post code[6]

We first extract the all the suburbs of Canberra along with their postalcodes by web scraping the data given at [7]. After collecting postal codes of all suburbs we use the GeoCoder API[8]t to get the latitiude and longitude for each suburb in Canberra. The data from the two aforementioned steps are combined to form a single table with suburb names, postal code and geo locations. Next, we traverse through the table to extract a list of all venues near the suburbs one by one and store in this in a new table with name and category of the venue combined with the name of the suburb. 

In our next step we aggregate the venues based on the suburbs to create one row for each suburb that list the frequqncy of all different venue categories. Finally, with help of SQM Research[6] data we combine the frequency of venue category data in a suburb with the avergae rent for an unit in the specific area. We now use this aggregated data to make decisions and rank areas as per individual's preferences. 

### <span style="color: slategrey;"> Methodology </span>

Data extracted from sources: [7], [8], [5] and [6] are stored in my repository for ease of access and fast iterations over data to repeat experiments. In total I extracted 143 suburbs for Canberra from[7] but it had three duplicates, making total suburbs in Canberra to 140. However, on combining data from [8] only 137 suburbs had their latitiude/longitude information available. Hence I started working on 137 Canberra Suburbs. The final information looked like the table below.

![/images/unclustered_image](table_suburbs.png)

Moving furhter I utilized the folium[9] library to viualize the suburbs in Canberra that looked as the picture below. This visualization made it clear that choosing suburbs in Canberra is really difficult task as they are clustered evenly and very compactly around Canberra, making any choice very difficult.

![/images/unclustered_image](Canberra_Suburbs.png)

Moving ahead I now utilized the foursquare[5] api to collect data about various venues in suburbs in Canberra. While exploring venues inorder to satisfy FourSquare API limits I had put rate limit to 100 and searched for venues in 1.5km range of each suburb. This exploration gave in in total of 3294 venues with 212 different venue categories. In next steps I used this venues as features to cluster suburbs in Canberra and find out the best suburb as per amenities available in Canberra. Finally I combine this clutred data with prices to chose the suburb region with least prices and better ammenities. The final dataframe I worked on had only 134 suburbs as some suburbs didn't had any venues listed ('Top Naas', 'Majura', 'Uriarra Forest'). The dataframe looked like as below.

![/images/unclustered_image](table_final.png)

### <span style="color: slategrey;"> Results </span>

Figure below shows the suburbs of Canberra with most number of venues listed (note number of venues rate limited to 100). As it can be seen the suburbs in central, iiner north and inner south are the ones with highest numbe of venues. Also quite surprisingly, the venues tail of very rapuldy after top 20 suburbs, inidicating most suburbs of Canberra are  not that well densely developed. 

![/images/unclustered_image](Canberra_Suburb_Venues.png)

Now in order to cluster suburbs in Canberra, I first use the elbow method to determine the best numbe rof clusters. As seen from the figure below, the best number of lcuster is 10.

![/images/unclustered_image](Canberra_cluster_numbers.png)

Once we have identified the number of clusters, we cluster Canberra into 10 different clusters as depicted by the map produced from folium below. Again, we can see most of the clusters are well coonected and contious regions. Apart from few clusters being solitay points and few being a surrounded by a bigger cluster.

![/images/unclustered_image](Canberra_Suburbs_clusters.png)

Finally, our anaysis of top venues in this clusters revel that only clusters with id 2(royal blue),3 (dark cyan),7 (green), and 4 (cyan) are sutiable for people with kids as they are the only suburbs with supermarket, grocery store, resturants, park and other helath amenities present.

### <span style="color: slategrey;"> Discussion </span>

After analyzing our data we realized only few suburbs are feasible places for a new couple to move in Canberra. Finally, after analyzing places we realised Suburb with id 2 (royal blue) is the best suburb in canberra for couples as it has all essentiall amenities as well as very moderate rent of \$451 for a two bedroom apartment. Another positive point for this suburb is that it is close to the city center and other center locations like Belconen and Gungahlin. Hence, the suburb has good prices alongwith a healthy outreach to various other suburbs. The map of suggested suburbs is given below

![/images/unclustered_image](Canberra_Suburb_best.png)

### <span style="color: slategrey;"> Conclusion </span>

In our report we have presented a way of combining offline(postal data and rental data) with various online locatiions data: venues from foursquare and latitude/longitude data from Geocode, to gather information about suburbs in Canberra for moving. Our approach albeit simple but makes attempt towrds helping people to make informed choices. A further extension of this work could be taking the previous information of the person mpving into cosideration before making choices.  

### <span style="color: slategrey;"> References </span>

1. https://www.allhomes.com.au/news/canberra-rents-continue-to-rise-still-most-expensive-capital-city-to-rent-a-house-817535/
2. https://www.canberratimes.com.au/story/6252699/why-has-renting-become-so-expensive-in-the-capital/
3. https://www.canberratimes.com.au/story/6434518/satisfaction-with-transport-network-down/
4. https://www.canberratimes.com.au/story/5992052/canberra-population-sprints-past-420000-new-figures-show/
5. https://developer.foursquare.com/
6. https://sqmresearch.com.au/weekly-rents.php?
7. https://worldpostalcode.com/australia/australian-capital-territory/canberra
8. https://geocoder.readthedocs.io/
9. https://python-visualization.github.io/folium/
