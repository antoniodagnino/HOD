# Unleashing the Dragon: Power BI and Python converge in an epic Battle Simulator Showcase

As House of the Dragon sets the stage for epic tales in the world of Westeros, imagine wielding the power of data to create a dragon battle simulator where we compare dragon's attributes, and determine the probability of victory in a fair one-on-one fight. This article unveils potential outcomes from encounters we might see in HOD Season 2 using a video game-like interface to select dragons and analyze the results from these battles. Do not worry about spoilers though. I won't reveal the real outcomes from George R. R. Martin books. This is just a battle simulator developed by collecting data from fanpages and social media with few details from the books. Overall, the main objective of the dragon battle simulator is to explain and implement basic data analytics methods like Normalization and apply weights to attributes using Python before showing the results through Power BI reports. 

![Battle Simulator](https://github.com/antoniodagnino/HOD/assets/76269794/a3f186e9-b839-46a2-a075-7291ac44cb47)

[Power BI dragon battle simulator link here!](https://app.powerbi.com/view?r=eyJrIjoiOWM5N2IxYTctMzkxOS00NmFhLTk3OTgtNDU1ODcxN2IyNGI0IiwidCI6IjAyOTczNGNmLWU2NzEtNGVjZS1hMzk4LWU5YTYzMzZkMmY1MSJ9)

<iframe title="HOD Project" width="600" height="373.5" src="https://app.powerbi.com/view?r=eyJrIjoiOWM5N2IxYTctMzkxOS00NmFhLTk3OTgtNDU1ODcxN2IyNGI0IiwidCI6IjAyOTczNGNmLWU2NzEtNGVjZS1hMzk4LWU5YTYzMzZkMmY1MSJ9&embedImagePlaceholder=true" frameborder="0" allowFullScreen="true"></iframe>



## **The Data**

After reading relevant details of some of the most popular dragons from the Game of Thrones prequel, I curated a database that contains 12 dragons characterized by attributes that provide insights into their capabilities and serve as the foundation for assessing the likelihood of a dragon's success in a hypothetical battle using the Power BI battle simulator. These attributes encompass age (years), size (ft), weight (lbs), speed (mph), strenght (lbf), battle experience (% of life) and fitness condition (%). However, these attributes have different physical measurements that require the data to be transformed in order to make a fair comparison. 

![The Data](https://github.com/antoniodagnino/HOD/assets/76269794/251da200-5e0d-45ce-a929-8281da064f14)



The data transformation consists of a 4 step process: data normalization, Apply weights, calculate the sum of all attributes transformed and determination of the probability of victory for each dragon in every potential fight.


**Normalization:**
Normalization is data pre-processing technique used to transform the values of features or variables in a dataset to a similar scale. The purpose is to ensure that all features contribute equally to the model and to avoid the domination of features with larger values. This well-known method is commonly used in the following scenarios:
Compare physical measurements like height and weight.
Compare customer ratings. For example, comparing ratings between 2 websites with different scales (one website with 5 star rating while the other has a 1 to 10 scale rating)
Compare financial metrics: Revenue in dollars, profit margin in percentage
In this project, we employ a Python library known as scikit-learn, opting specially for the MinMaxScaler object to carry out attribute transformation. MinMaxScaler  converts all attributes in a common scale, ranging from 0 to 1. As an illustration, the dragon with the maximum speed will be assigned a value of 1 for speed, whereas the lowest speed will correspond to 0. The normalization is applied uniformly across all the attributes within the dataset. A snippet of Python code for this process is shown below. For those interested in exploring the complete Python code and dataset, check the Jupyter Notebook in this repository.

```
from sklearn.preprocessing import MinMaxScaler #Import library

scaler = MinMaxScaler()  #create normalization object
scaled_values = scaler.fit_transform(df) #apply normalization to data "df"
```


**Weights:**
Once the data is normalized, all attributes carry an equal weight in battles, which is not entirely accurate. For instance, the impact of dragon weight should differ from that of battle experience. Often, battle experience holds more sway than the weight alone. Consequently, I assigned specific weights to each feature, distributing their overall influence as follows:
age = 10%
size = 14%
weight = 10%
speed = 10%
strength = 16%
battle experience = 22%
fitness condition = 18%
This distribution ensures that the attributes collectively contribute to the battle dynamics in a manner that closely mirrors reality. It's possible to experiment with these proportions to fine-tune the outcomes of dragon battles further.


**Computation of dragon's coefficient:**
The dragon's coefficient is derived from the summation of all dragon attributes, factoring in their respective weights. These coefficients serve as the basis for assessing the overall performance of a dragon based on its skills. The formula is as follows:

```
Coefficient = (age*W1) +(size*W2) +(weight*W3) +(speed*W4) +(strength*W5) +(battleExperience*W6) +(fitness*w7)
```

![Heatmap](https://github.com/antoniodagnino/HOD/assets/76269794/ba3f2d2a-0a1c-4df3-8443-be42aaff59e1)


**Probability of Victory (Power BI Measures):**
The probability of victory relies on the coefficients of the two dragons chosen for the battle. Initially, we choose the two dragons, and then Power BI will aggregate their two coefficients, symbolizing a total probability of victory at 100%. Subsequently, we distribute the percentage between the dragons using the following formulas:

```
Dragon1_probability = coef_dragon1 / (coef_dragon1 + coef_dragon2)
Dragon2_probability = coef_dragon2 / (coef_dragon1 + coef_dragon2)
```

## **Data Visualization**


The dragon battle simulator integrates various visuals and tools accessible within Power BI to render the fantastic video game-style selection visualization seen below. Positioned on the left and right sides we have bookmark navigator buttons, KPI cards and illustrations that allow you to select, analyze and see the dragons along with their respective attributes. Then, in between the dragons you will see the dragon names currently selected with their probability of victory. The probability fluctuates depending on the dragons been showcased. The Power BI tools used for this report include calculated tables, cards, slicers, measures, bookmarks and illustrations.


![Battle Simulator with elements](https://github.com/antoniodagnino/HOD/assets/76269794/03ea506c-c7e8-40f3-81b0-1ecdc4664c78)

**Calculated Tables:** We created a copy of the HOD database without relationships so we are flexible to slice or filter the database for the 2 dragons we will select. Simply put, the dragon on the left utilizes the original HOD table, while the dragon on the right employs a duplicated copy.

**Slicers:** There are 2 hidden slicers in the simulator that filter the data to display only the attributes of the selected dragons. This allows showing the right attributes for the dragon as well as the dynamic calculation of  probability of victory. There is one slicer per dragon database.

**Measures:** I used multiple measures throughout the development to display the attribute bars with loading effect, show dragon's names, dragon rider's names and the calculation of probability of victory per dragon. An example of how the "size" load bar was made is shown in the code below:

```
SizeBar = 
VAR Bar_Total = 10
VAR Bar_Fill =  ROUNDDOWN(SELECTEDVALUE(HOD[Size (ft)])*10/CALCULATE(MAX(HOD[Size (ft)]),ALL(HOD)),0)
VAR Bar_Empty = Bar_Total-Bar_Fill
VAR Fill_icon = "▰"
VAR Empty_icon = "▱"
VAR DataBar = 
    REPT(Fill_icon, Bar_Fill) &
    REPT(Empty_icon,Bar_Empty)

RETURN
DataBar
```

The "size" text right below the load bar is also done with a measure with the following code:

```
Size = "Size: " & SELECTEDVALUE(HOD[Size (ft)]) & "ft"
```


**(New) Card:**  The new card was released in November 2023 Power BI update and we used it to display dragon's attributes in numbers and with the loading effect bars. The load bars are placed in the "data" section when building a visual so they become "callout values". After that, the text version of the attributes are put in "reference labels" option found in "Format" pane.

![New card build and format visual](https://github.com/antoniodagnino/HOD/assets/76269794/d182b3f1-933e-4707-b2b2-0f6f951eda19)

As reference, you can click [here](https://www.youtube.com/watch?v=KgkOY4-sUUQ&t=1121s) and watch the full explanation of how to work with this new card and create bars with loading effects.

**Card:** The traditional cards are used to display dragon names and dragon rider names. You need to assign a measure to the card and assign a measure that will be influenced by the slicer based on the dragon selected. We use the "SELECTEDVALUE" function inside the measure to do that.

```
DragonRider = "Dragon Rider: " & SELECTEDVALUE(HOD[Dragon Rider])
```

**Images:** All dragon illustrations were obtained from fanpages and social media public posts. If you're a HOD fan and liked the dragon arts, please give credit to [Siosin](https://www.instagram.com/siosin_/), as he makes a great job with the accurate HOD drawings. Morever, the backgrounds are made using PowerPoint by adjusting background colors and applying gradients.

**Bookmark Navigator:** The bookmarks help putting altogether, synchronize slicers, data and images to allow Power BI show only what you want to see and hide the rest. We one bookmark per dragon and after all bookmarks are created, we use the Bookmark Navigator to allow users select the dragon they want.

![Bookmarks and Selection Panels](https://github.com/antoniodagnino/HOD/assets/76269794/e0293c0a-44ca-44f2-8dc7-326e39ba4d94)

**Page Navigator:** This allows to change between dark and light background versions of the dragon battle simulator. We have 2 pages showing the same data but different background and text colors for users to appreciate the visuals as they want to.


## **Conclusion**
In conclusion, the exploration of a dragon battle simulator within the realm of House of the Dragon opens up thrilling possibilities. By harnessing the power of data, this simulator offers enthusiasts the chance to delve into hypothetical one-on-one battles, comparing dragon attributes and calculating the probability of victory. The primary goal of this project is to introduce and implement fundamental data analytics methods such as Normalization and attribute weighting using Python, culminating in insightful Power BI reports showcasing the simulated results. I trust that the fusion of data analytics and entertaining content contributes in making learning a more enjoyable and comprehensible experience.












