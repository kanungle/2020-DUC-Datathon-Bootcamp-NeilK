# DUC Datathon Bootcamp with TIBCO Spotfire

Welcome to the DUC Datathon 2020 Intermediate Level Bootcamp for TIBCO Spotfire :bar_chart:! Follow along with this guide to learn about geoanalytics, data functions, machine learning, visualization best practices, and more.




### Introduction

My name is Neil Kanungo, a Data Scientist with TIBCO Software. You might know me from [Dr. Spotfire](https://community.tibco.com/wiki/doctor-spotfire-office-hours) :grin:! If you haven't already, I highly recommend you subscribe to our [YouTube channel](https://www.youtube.com/channel/UCx3agqDZLbfrHNDUaxr0CXA) to get regular Spotfire video tips, and [register](https://www.tibco.com/events/dr-spotfire-office-hours) for one of our monthly live sessions. There's always tons to learn with TIBCO Spotfire and I aim to teach you all the newest tricks :cowboy_hat_face:

The bootcamp in this repo covers three (3) exercises to help intermediate users:

- [Exercise 1 : Geospatial Analysis](https://github.com/kanungle/2020-DUC-Datathon-Bootcamp-NeilK/tree/master/Exercise%201%20-%20Geospatial%20Analysis)
- [Exercise 2 : Regression Analysis](https://github.com/kanungle/2020-DUC-Datathon-Bootcamp-NeilK/tree/master/Exercise%202%20-%20Regression%20Analysis)
- [Exercise 3 : Styling Tips](https://github.com/kanungle/2020-DUC-Datathon-Bootcamp-NeilK/tree/master/Exercise%203%20-%20Styling%20Tips)

You can find the files for each exercise in separate folders of this directory, or simply linked above.




### Exercise 1: Geospatial Analysis

![Overview](https://github.com/kanungle/2020-DUC-Datathon-Bootcamp-NeilK/blob/master/images/Ex%201%20-%20Overview.png)

__:arrow_right: [Download Example DXP](https://github.com/kanungle/2020-DUC-Datathon-Bootcamp-NeilK/blob/master/Exercise%201/Exercise%201%20-%20Geospatial%20Analysis.dxp)__

Using sophisticated Machine Learning and Geospatial Analysis can be easy in TIBCO Spotfire! In this thorough exercise, you will learn how to spatially interpolate values, generalize values with contours, cluster with unsupervised machine learning, merge points and geometries, and determine important variables. This example uses Isopach data to start, and goes through each of the aforementioned analyses with powerful Spotfire Data Functions written in R (TERR).

#### Step 1: Spatial Interpolation

TERR Expression Code for Spatial Interpolation:
```
# Interpolate missing values at points on a surface using local polynomial regression.

# The first argument is a column of real number values indicating x coordinates or longitudes. 
# The second argument is a column of real number values indicating y coordinates or latitudes. 
# The third argument is a column of real number or integer valued data containing missing values. 
# The fourth argument is a scalar real number value controlling the degree of smoothness.

# Example:
# InterpolateSmoothSurface([x], [y], [z], 0.5)

x <- input1
y <- input2
z <- input3
smooth.scale <- input4[1]

xyzdata = data.frame( x=x, y=y, z=z )
bad = is.na(x) | is.na(x) | is.na(z)
xyzdata = xyzdata[!bad,]

xyzdata.lo <- try(loess(z~x*y, data=xyzdata, span=smooth.scale, na.action=na.exclude))

xyzdata.lo.pred <- predict( xyzdata.lo, newdata=data.frame(x=x, y=y))

output <- xyzdata.lo.pred

```

Data Functions:
- [Contour Plot](https://community.tibco.com/modules/map-contour-plot-data-function-tibco-spotfire)
- [Voronoi Polygons](https://community.tibco.com/modules/voronoi-polygons-tibco-spotfire)
- [Points-in-Polygons](https://community.tibco.com/modules/points-polygons-data-function-tibco-spotfire)

Also see: 
- [TIBCO Exchange](https://community.tibco.com/exchange/product/541)
- [Spotfire Geoanalytics Home Page](https://community.tibco.com/wiki/tibco-spotfire-location-analytics-mapping-geoanalytics-and-spatial-statistics)
- [Custom Map Backgrounds](https://community.tibco.com/wiki/geoanalytics-resources)

Shapefiles:
- Download [Alberta Natural Regions and Subregions Shapefile](https://www.albertaparks.ca/media/429607/natural_regions_subregions_of_alberta.zip) (source: Alberta Parks)
- Download [Alberta Play Outline Shapefile](https://www.aer.ca/documents/catalog/PlayWorkbookListArea.zip) (source: Alberta Energy Regulator)




### Exercise 2: Regression Analysis

![Overview](https://github.com/kanungle/2020-DUC-Datathon-Bootcamp-NeilK/blob/master/images/Ex2%20-%20Results.png)

__:arrow_right: [Download Example DXP](https://github.com/kanungle/2020-DUC-Datathon-Bootcamp-NeilK/blob/master/Exercise%202%20-%20Regression%20Analysis/Exercise%202%20-%20Regression%20Analysis.dxp)__

TIBCO Spotifre has many built-in tools to help you with Machine Learning and Statistical Analysis right out of the box :package:! In this quick example, you'll walk through some basic Well Production Data for the Bakken Formation, and determine the top predictors for best production without writing any code.

Step-by-step:
  1. Download and open the DXP provided above
  2. Go to Tools > Regression Modeling, and select:
    - Response column: Production at 12 Months (BOE)
	- Predictor columns: all the other columns except Latitude and Longitude
  3. Then click OK and review the output.
  4. Enlarge the variable importance plot to show which columns are predictive of Production at 12 Months (BOE). 
    - What are the most important variables? 
	- How do the residuals compare to fitted values? 
	- Can you use Cook's Distance to identify outliers?




### Exercise 3: Styling Tips

![Overview](https://github.com/kanungle/2020-DUC-Datathon-Bootcamp-NeilK/blob/master/images/Ex3%20-%20Color%20Guidance.png)

__:arrow_right: [Download Example DXP]()__

Styling visualizations and dashboards is not just for aesthetics, it's for _communication_. Clearly assembled visualizations help communicate data insights to your audience, which is the core purpose of data visualization.

The topic itself is quite broad by nature, but I have compiled some Spotfire resources here for you to reference and an exercise to follow along with. We will go over these during the live Bootcamp session. (If you are participating in the Data Visualization Competition, this topic is very important!!)

Video Tips:
 - [Styling and Configuration Overview (11:22)](https://youtu.be/1pfGb-cHrgc)
 - [Using Color Effectively (8:27)](https://youtu.be/6pvYRdPRQv8)
 - [Build a User Experience (12:04)](https://youtu.be/nQ6w7iC-gt4)

Cheatsheets:
 - [Choosing the Right Chart](http://community.tibco.com/sites/default/files/choosing_the_right_chart.pdf)
 - [Color Scheme Guidance](https://community.tibco.com/sites/default/files/color_scheme_guidance_1.pdf)
 - [More cheatsheets](https://community.tibco.com/wiki/spotfire-cheatsheets)
 
 Color Resources:
  - [Adobe Color Wheel](https://color.adobe.com/create/color-wheel)
  - [DataViz Color Palette Generator](https://learnui.design/tools/data-color-picker.html)
  - [ESRI Color Ramps](https://developers.arcgis.com/javascript/latest/guide/esri-color-ramps/)
  - [ColorBrewer2](https://colorbrewer2.org/)





### Connect with others!

Data Analytics is a constantly-changing, fast-moving journey. To continue learning with Spotfire, consider joining one of our user groups to network with your peers and stay up-to-date on the latest!

__User Groups:__
- [Facebook](https://www.facebook.com/groups/651751391967838)
- [Reddit](https://www.reddit.com/r/spotfire/)
- [Twitter](https://twitter.com/DrSpotfire)
- [LinkedIn](https://www.linkedin.com/groups/12253057/)
- [Instagram](https://www.instagram.com/drspotfire/)

Also, if you are new to Spotifre and want to learn the basics from an organized curriculum, you can visit the [Spotfire Enablement Hub (free)](https://community.tibco.com/wiki/spotfire-enablement-hub) or [TIBCO Academy (paid)](https://academy.tibco.com/tibco/learn/home).

See you out there Datathoner! :v: