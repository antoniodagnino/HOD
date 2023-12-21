# Unleashing the Dragon: Power BI and Python converge in an epic Battle Simulator Showcase

As House of the Dragon sets the stage for epic tales in the world of Westeros, imagine wielding the power of data to create a dragon battle simulator where we compare dragon's attributes, and determine the probability of victory in a fair one-on-one fight. This article unveils potential outcomes from encounters we might see in HOD Season 2 using a video game-like interface to select dragons and analyze the results from these battles. Do not worry about spoilers though. I won't reveal the real outcomes from George R. R. Martin books. This is just a battle simulator developed by collecting data from fanpages and social media with few details from the books. Overall, the main objective of the dragon battle simulator is to explain and implement basic data analytics methods like Normalization and apply weights to attributes using Python before showing the results through Power BI reports. 


Power BI link: https://app.powerbi.com/view?r=eyJrIjoiOWM5N2IxYTctMzkxOS00NmFhLTk3OTgtNDU1ODcxN2IyNGI0IiwidCI6IjAyOTczNGNmLWU2NzEtNGVjZS1hMzk4LWU5YTYzMzZkMmY1MSJ9

- The Data

After reading relevant details of some of the most popular dragons from the Game of Thrones prequel, I curated a database that contains 12 dragons characterized by attributes that provide insights into their capabilities and serve as the foundation for assessing the likelihood of a dragon's success in a hypothetical battle using the Power BI battle simulator. These attributes encompass age (years), size (ft), weight (lbs), speed (mph), strenght (lbf), battle experience (% of life) and fitness condition (%). However, these attributes have different physical measurements that require the data to be transformed in order to make a fair comparison. 

The data transformation consists of a 4 step process: data normalization, Apply weights, calculate the sum of all attributes transformed and determination of the probability of victory for each dragon in every potential fight.

Normalization
Normalization is data pre-processing technique used to transform the values of features or variables in a dataset to a similar scale. The purpose is to ensure that all features contribute equally to the model and to avoid the domination of features with larger values. This well-known method is commonly used in the following scenarios:
Compare physical measurements like height and weight.
Compare customer ratings. For example, comparing ratings between 2 websites with different scales (one website with 5 star rating while the other has a 1 to 10 scale rating)
Compare financial metrics: Revenue in dollars, profit margin in percentage
In this project, we employ a Python library known as scikit-learn, opting specially for the MinMaxScaler object to carry out attribute transformation. MinMaxScaler  converts all attributes in a common scale, ranging from 0 to 1. As an illustration, the dragon with the maximum speed will be assigned a value of 1 for speed, whereas the lowest speed will correspond to 0. The normalization is applied uniformly across all the attributes within the dataset.


Weights
Once the data is normalized, all attributes carry an equal weight in battles, which is not entirely accurate. For instance, the impact of dragon weight should differ from that of battle experience. Often, battle experience holds more sway than the weight alone. Consequently, I assigned specific weights to each feature, distributing their overall influence as follows:
age = 10%
size = 14%
weight = 10%
speed = 10%
strength = 16%
battle experience = 22%
fitness condition = 18%
This distribution ensures that the attributes collectively contribute to the battle dynamics in a manner that closely mirrors reality. It's possible to experiment with these proportions to fine-tune the outcomes of dragon battles further.


Computation of dragon's coefficient
The dragon's coefficient is derived from the summation of all dragon attributes, factoring in their respective weights. These coefficients serve as the basis for assessing the overall performance of a dragon based on its skills.


Probability of Victory (Power BI Measures)
The probability of victory relies on the coefficients of the two dragons chosen for the battle. Initially, we choose the two dragons, and then Power BI will aggregate their two coefficients, symbolizing a total probability of victory at 100%. Subsequently, we distribute the percentage between the dragons using the following formulas:
Dragon1_probability = coef_dragon1 / (coef_dragon1 + coef_dragon2)
Dragon2_probability = coef_dragon2 / (coef_dragon1 + coef_dragon2)





