# DUC Datathon Bootcamp with TIBCO Spotfire

Welcome to the DUC Datathon 2020 Intermediate Level Bootcamp for TIBCO Spotfire :bar_chart:! Follow along with this guide to learn about geoanalytics, data functions, machine learning, visualization best practices, and more.




### Introduction

Hi! My name is Neil Kanungo and I'm a Data Scientist with TIBCO Software. You might know me from [Dr. Spotfire](https://community.tibco.com/wiki/doctor-spotfire-office-hours) :grin:! If you haven't heard of Dr. Spotfire, I highly recommend you subscribe to our [YouTube channel](https://www.youtube.com/channel/UCx3agqDZLbfrHNDUaxr0CXA) to get regular Spotfire video tips, and [register](https://www.tibco.com/events/dr-spotfire-office-hours) for one of our monthly live sessions. There's always tons to learn with TIBCO Spotfire and I aim to teach you all the newest tricks :cowboy_hat_face:

The bootcamp in this repo covers three (3) exercises to help intermediate users:

- [Exercise 1 : Geospatial Analysis](https://github.com/kanungle/2020-DUC-Datathon-Bootcamp-NeilK/tree/master/Exercise%201%20-%20Geospatial%20Analysis)
- [Exercise 2 : Regression Analysis](https://github.com/kanungle/2020-DUC-Datathon-Bootcamp-NeilK/tree/master/Exercise%202%20-%20Regression%20Analysis)
- [Exercise 3 : Styling Tips](https://github.com/kanungle/2020-DUC-Datathon-Bootcamp-NeilK/tree/master/Exercise%203%20-%20Styling%20Tips)

You can find the files for each exercise in separate folders of this directory, or simply linked above.




### Exercise 1: Geospatial Analysis

__:arrow_right: [Download Example DXP](https://github.com/kanungle/2020-DUC-Datathon-Bootcamp-NeilK/blob/master/Exercise%201/Exercise%201%20-%20Geospatial%20Analysis.dxp)__

![Overview](https://github.com/kanungle/2020-DUC-Datathon-Bootcamp-NeilK/blob/master/images/Ex%201%20-%20Overview.png)

Using sophisticated Machine Learning and Geospatial Analysis can be easy in Spotfire! In this thorough exercise, you will learn how to spatially interpolate values, generalize values with contours, cluster with unsupervised machine learning, merge points and geometries, and determine important variables. Download the Example DXP above to get started. This DXP uses isopach (thickness measurment) data, and goes through each of the aforementioned analyses with powerful Spotfire Data Functions written in R (TERR).

#### Step 1: Spatial Interpolation Function
Explore the data in the map. Select different geological layers from the drop down. Settle on "Lower Mannville Isopach" for this example.

Also turn on/off the layers for "Non-Missing Values" and "All Values". The latter shows coordinates for every point but doesn't have measurements for every coordinate and at every layer. Let's interpolate missing values for the "Lower Mannville Isopach" using a TERR Expression.

Go to _Data > Data Function Properties > Expression Functions_. You will see the "InterpolateSmoothedSurface" using the code below. Observe the code and configuration here.

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

This function takes x,y coordinates, some measurement as z, and a smoothing parameter 'theta' to interpolate missing values using local polynomial regression (Loess smoothing). We'll apply this in Step 2.

#### Step 2: Apply Interpolation to Color Axis

In your map layers, go to the _Color By_ options for the "All Values" layer. Right-click the column selector and apply the following statement:

```
InterpolateSmoothedSurface([lon],[lat],[mani2],${theta})
```

The InterpolateSmoothSurface function shows up like any other Spotfire function! Here we use "mani2" as the z-value which corresponds to our geological layer from the drop down, and we use "theta" as a Document Property for the smoothing parameter. The Document Property value may be adjusted from the slider at the top of the dashboard page, allowing a dynamic recalculation of interpolation and smoothing.

Before leaving the "Color By" configuration, set the color theme from the Document Color themes as "Continous - YellowPink" (find this in the document icon of "Color By" menu)

#### Step 3: Download Free Data Functions from TIBCO Exchange

The following three Data Functions should be downloaded for free directly from the TIBCO Exchange:
- [Contour Plot](https://community.tibco.com/modules/map-contour-plot-data-function-tibco-spotfire)
- [Voronoi Polygons](https://community.tibco.com/modules/voronoi-polygons-tibco-spotfire)
- [Points-in-Polygons](https://community.tibco.com/modules/points-polygons-data-function-tibco-spotfire)

Once you download each of these, you should unzip them on your local drive.

The TIBCO Community has a wealth of resources and a very active Q&A section. You may choose to bookmark the following pages which we'll discuss throughout the exercise:
- [TIBCO Exchange](https://community.tibco.com/exchange/product/541)
- [Spotfire Geoanalytics Home Page](https://community.tibco.com/wiki/tibco-spotfire-location-analytics-mapping-geoanalytics-and-spatial-statistics)
- [Custom Map Backgrounds](https://community.tibco.com/wiki/geoanalytics-resources)

#### Step 4: Spatial Generalization with Contour Plots

Back in our example, let's generalize the individual points to contour lines which fit the data into 2-dimensional trends using the same local polynomial smoothing calculation. 

To apply this R script using the Spotfire TERR engine, go to _Data > Data Function Properties > Import_ and select the "contour_plotSFD" file. Observe the script (no need to change this!) then hit the "Run" button. Now we will set parameters:

- x = lon column of isopach table
- y = lat column of isopach table
- z = mani2 column of isopach table
- smoothing.value = theta as Document Property

For Output, simply select Data Table and allow it to write to a new table "Contours."

Be sure to click the checkbox "Refresh function automatically" before closing.

Now in your analysis, you may add a Feature Layer to your map for the "Contours" data table and "Feature By" row number. You may choose to color this by "Level" and with the same document color scheme "Continuous - YellowPink".

As you move the smoothing slider around, you will see both interpolated values and generalized contours recalculate dynamically! This gives you a more efficient and complete way to view and analyze the original source data.

#### Step 5: Clustering with Voronoi Polygons

Next, lets use unsupervised machine learning to cluster these points into polygon geometries. We'll do this by using the Voronoi Polygon Data Function and import this SFD file the same way we did the contours in Step 4.

For our inputs:
- Xcoordinate = lon column of isopach table
- Ycoordinate = lat column of isopach table
- Reduction factor = 100 (set as Value)
- DataColumn = mani2 column of isopach table

For Output, just choose "Data Table" for the Voronoi option. The other outputs are not needed here.

(Note: a Reduction Factor of 100 was used since we have over 14,000 points and we only want a few polygons)

After the polygons calculate, you can add them as a Feature Layer and style it just as you did the Contour Plot layer. The Polygons give us a generalized area calculation for the underlying isopach geology, and is clustered by the density of points. Each polygon has a mani2 value which corresponds to the average of all the mani2 points in that polygon. This helps us break up the thousands of points into smaller, generalized chunks for further analysis.

#### Step 6: Geofencing with Points-in-Polygons

We want to know how Production relates to this geology, so we need a way to analyze this data together. A small sample of wells has been included in the table "Wells" which we will intersect with the Voronoi Polygon data. The "Natural Regions and Subregions" data has been included from a publicly available shapefile. you can find this shapefile below. I've also included another useful shapefile (not used here) that gives different Alberta Plays for various geological strata.

Shapefiles:
- [Alberta Natural Regions and Subregions Shapefile](https://www.albertaparks.ca/media/429607/natural_regions_subregions_of_alberta.zip) (source: Alberta Parks)
- [Alberta Play Outline Shapefile](https://www.aer.ca/documents/catalog/PlayWorkbookListArea.zip) (source: Alberta Energy Regulator)

To try this out, import the Points-in-Polygons SFD file and apply it between this first shapefile and the Isopach data. For inputs:

- Geometry = GEOMETRY column of shapefile
- Id = OBJECTID column of shapefile
- x = lon column of isopach table
- y = lat column of isopach table

For Outputs:
- Id = column on Isopach table

This now took each OBJECTID value for a shape, and applied that value to each individual row on the isopach table for points. Therefore each point is now mapped to a shape object. You can observe this by coloring the isopach table layer "All values" by column "Id"

#### Step 7: Combining Geology and Production Data

The above Points-in-Polygons example was a simple way to show geofencing. Let's now use it to link the Well Production data and the generalized Voronoi Polygons with geology data.

Follow the same process as Step 6, except now instead of using GEOMETRY and OBJECTID from the shapefile, use Geometry and tileID from the Voronoi Polygon table. Instead of x, y from the Isopach table, instead use Longitude and Latitude from the Wells table.

You should have a new column in the Wells table named "Id" corresponding to Voronoi polygon "tileID".

Now you simply need to join to the two tables so you can bring mani2 values to the Wells data. First you'll need to "Freeze" this Id column by going to _Data > Column Properties > Id > Freeze Column_. This prevents the column from changing dynamically with changes to the data function, and makes our upcoming join more stable for Spotfire. We want to join "Id" of the Wells table to "tileID" of the Voronoi Polygon table so we can bring in the mani2 data from Voronoi. We do this through the "Add Columns" capability in the Spotfire Data Canvas for the Wells table, and only include the "mani2" column to add, and choose "Left Single Match" as the join type.

Now that the data is combined, we can open our Data Panel and select the Production columns as our target variable and see the top predictors for that production value. We can see how isopach data (mani2) relates in the various charts!

#### Conclusion:

This was a very simple example with some sample wells, but this process can be applied to real-world wells with more rich data to do a similar analysis, and can generally be applied whenever you want to analyze individual point data and it's relationship to other point data.

:arrow_right: [Go to Exercise](https://github.com/kanungle/2020-DUC-Datathon-Bootcamp-NeilK/tree/master/Exercise%201%20-%20Geospatial%20Analysis)




### Exercise 2: Regression Analysis

__:arrow_right: [Download Example DXP](https://github.com/kanungle/2020-DUC-Datathon-Bootcamp-NeilK/blob/master/Exercise%202%20-%20Regression%20Analysis/Exercise%202%20-%20Regression%20Analysis.dxp)__

![Overview](https://github.com/kanungle/2020-DUC-Datathon-Bootcamp-NeilK/blob/master/images/Ex2%20-%20Results.png)

TIBCO Spotifre has many tools to help you with Machine Learning and Statistical Analysis right out of the box :package:! In this quick example, you'll walk through some basic Well Production Data for the Bakken Formation, and determine the top predictors for best production without writing any code.

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

:arrow_right: [Go to Exercise](https://github.com/kanungle/2020-DUC-Datathon-Bootcamp-NeilK/tree/master/Exercise%203%20-%20Styling%20Tips)




### Exercise 3: Styling Tips

__:arrow_right: [Download Example DXP](https://github.com/kanungle/2020-DUC-Datathon-Bootcamp-NeilK/blob/master/Exercise%203%20-%20Styling%20Tips/Exercise%203%20-%20Styling%20Guidance.dxp)__

![Overview](https://github.com/kanungle/2020-DUC-Datathon-Bootcamp-NeilK/blob/master/images/Ex3%20-%20Color%20Guidance.png)

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


:arrow_right: [Go to Exercise](https://github.com/kanungle/2020-DUC-Datathon-Bootcamp-NeilK/tree/master/Exercise%202%20-%20Regression%20Analysis)




### Connect with others!

Data Analytics is a constantly-changing, fast-moving journey. To continue learning with Spotfire, consider joining one of our user groups to network with your peers and stay up-to-date on the latest!

__Spotfire User Groups:__
- [Facebook](https://www.facebook.com/groups/651751391967838)
- [Reddit](https://www.reddit.com/r/spotfire/)
- [Twitter](https://twitter.com/DrSpotfire)
- [LinkedIn](https://www.linkedin.com/groups/12253057/)
- [Instagram](https://www.instagram.com/drspotfire/)

Also, if you are new to Spotifre and want to learn the basics from an organized curriculum, you can visit the [Spotfire Enablement Hub (free)](https://community.tibco.com/wiki/spotfire-enablement-hub) or [TIBCO Academy (paid)](https://academy.tibco.com/tibco/learn/home).

See you out there Datathoner! :v: