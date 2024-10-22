---
tags:
  - "#resource"
  - "#AAI-500-02"
  - "#statistics"
  - "#usd"
  - "#masters"
  - "#mestrado"
  - "#regression"
  - "#machineLearning"
  - "#games"
Created: 2024-10-20
Links:
  - "[[Using Linear Regression to Predict the Price of Magic The Gathering Cards]]"
  - "[[Inferential Statistics]]"
  - "[[Final Project Probability and Statistics Assignment]]"
  - "[[Linear Regression]]"
Area:
  - "[[Probabilty]]"
---
> [!hint] Card Price Prediction for Magic the gathering
Group 03 - Victor Hugo Germano and Victoria Dorn
Professor MSc. Leon Shpaner 
[Project page on Github](https://github.com/victorhg/aai-500-final-project/)


# Abstract

[[Magic The Gathering]] Card Game is one of the most famous card games in the world, with a thriving community of players, championships, physical and online games, and a big market for buying and selling cards worldwide. This article takes a deep dive approach using Exploration Data Analysis and [[Machine Learning]] techniques to present models of predicting prices based on a variety of information about the game. We begin by analyzing card characteristics and their relationship with the pricing information, generating a basis of predictors like rarity, availability, power, artist (to name a few) and our selected target of price information relative to a single day in the market. [[Linear Regression]], [[Ordinary Least Squares Regression]], [[Self Organizing Maps]] and [[Random Forest Regression]] were used and compared on their predictive capabilities using our model of numerical and categorical data, and using the Mean Squared Error and R-squared, concluding that EDH Ranking and EDH Saltiness play a significant role as price predictors, beyond external market factors. Future research could try to incorporate more information beyond Card attributes to improve the model.

Keywords: _regression, machine learning, price prediction, card games_

# Introduction


[[Magic the Gathering]] is one of the most famous card games in the world. First published in 1993 by Wizards of the Coast, it has become a benchmark of the modern fantasy games, getting more than 40 million players around the world (Schmidt., 2023).

With more than 23,000 unique cards, the game is a phenomenon, having worldwide championships with cash prizes up to $10 million. For example, The Las Vegas Grand Prix championship receives close to 11,000 participants every edition (Grand Prix, 2024).

The game also has a thriving primary and secondary market for cards. The rarest card in the game, the Black Lotus, has been sold for $600,000 on an auction, with some cards receiving even higher expected value. 

We believe that analyzing card assets in relation to their general prices in the secondary market can offer valuable insights to collectors. The primary object of this project is to statistically analyze the price variable against various feature variables using methods learned in this course, and employ regression processes to predict prices. 

We decided to apply multiple regression and classification techniques to allow us to predict prices based on the categorical data from Cards, using Linear Regression techniques and [[Ordinary Least Squares Regression]]. We will then employ a [[Self Organizing Maps]] (SOM) to assist with clustering and visualization. Ultimately, using [[Random Forest Regression]], we aim to compare some of the results obtained from the SOM with our own manual statistical evaluations and lay the groundwork for building trust in machine learning models for multidimensional datasets. Numerous fields can benefit from the application of simple machine learning algorithms such as SOMs.

Given the maturity of the Magic: The Gathering card market, we have identified an open-source dataset that includes current (or relatively recent) market prices. This comprehensive dataset will be our ground truth, rather than aggregating data from multiple marketplaces.	Subsequently, if time permits we would like to implement a predictive model for card price predictions using Card Attributes and Price Information from a single market day.

Secondary markets are influenced by many factors when pricing a card, many of which are external to the data used in the article, and that could be used in future research. Synergy between cards during gameplay, deck strategy in Tournaments, exclusive art and celebrity endorsement can affect price and will not be used in the analysis. 


![[Magic The Gathering#Overview of Magic The Gathering, the Game]]

# Data Cleaning and Preparation
For dataset selection, we found an existing open-source project, MTGJSON, that gears itself towards data aggregation and organization across all MTG play formats. They have APIs already set up to pull new information daily along with thorough documentation, which were the main deciding factors in selecting their datasets for use in this project. Specifically we utilized two main datasets for our analysis, one containing the general Card Attributes for all printed cards coupled with the second that contained card Pricing information on a single market day.

Before the data cleaning process, the Card dataset consisted of 97,145 individual card entries and the Price dataset consisted of 558,079 total entries from 91,302 unique card entries. Further examination of these datasets showed 25 possible variables for each card entry, some of which include Name, Game Attributes, Artist, Collection, EDH Ranking, Card Finish and a mix of other qualitative and quantitative variables, and 8 possible variables for each card entry, some of which include Currency and Price Provider. 

Next, we will delineate the steps undertaken to clean and prepare the dataset, to foster transparency in our analytical process as a whole. To streamline our evaluation and reduce complexity to our multidimensional dataset, we reviewed both datasets individually prior to merging them. First, we decided not to incorporate exchange rates and instead selected a single currency.  There were 432,688 USD entries and 125,391 EUR entries, so we selected USD for our price comparisons. Subsequently, we removed all constants from both datasets. This meant eliminating any column that contained only a single unique value throughout the data. The columns discarded included Currency (USD) and Data (2024-09-20) from the prices dataset. 

An initial comparison of our two datasets led us to determine that there were duplicate variables. Since our end goal with data cleaning was to create a comprehensive primary dataset we removed the Finishes, HasFoil, HasNonFoil, and SourceProducts column from the Card dataset since this information would be reflected more accurately in the Price dataset CardFinish variable. Additionally, we filled in the isReprint variable missing values with False since there were only True or NaN values on ingest. The Colors variables also had repeat permutations of string values, so we cleaned that up from 41 to 32 unique values. 

To create our primary dataset, we performed an inner join utilizing the uuid column, which contained a unique identification number for each individual card. Doing this ensured that only cards present in both datasets would be retained then we dropped missing values. This approach was chosen for our primary dataset to maintain the integrity of the data, especially so missing values did not skew results. The primary dataset enables a more comprehensive analysis, allowing us to examine more features and their corresponding correlations to the price of the card. It is composed of 30 variables and 17,628 total card entries.

Upon further evaluation of the columns we dropped, we decided to create a second, higher-fidelity dataset. Many variables that capture essential game mechanics were removed when we dropped missing values. To preserve card types beyond just Creature, it became necessary to retain the Power and Toughness attributes, which were among the missing data. Similarly, most Instant and Sorceries do not have Supertypes, so we opted to retain those missing values as well. Additionally, for the Color and ColorIdentity attributes, which had missing values, we choose to keep these as unique identifiers, such as “C” for colorless. 

This second dataset allows us to maintain the integrity of the dataset while ensuring that we capture the full range of card types and their characteristics. The second dataset contains 269,807 total card entries and 27 variables.

Given that we wanted to explore categorical variables in our analysis, the variables in both datasets were converted into numerical formats using label encoding. This transformation was applied to all columns, excluding uuid. By encoding these variables, we ensured that they could be effectively included in subsequent modeling, allowing us to explore their impact on card prices.

To further enhance the reliability of the experiment results, price outliers were identified and removed using the Interquartile Range method (Agresti & Kateri, 2021) . This method calculates the first and third quartiles to determine an acceptable range for data points. Any prices falling outside this range were considered outliers and excluded from further analysis. This step was critical in ensuring that extreme values did not distort the regression results, especially given that there were 2,414 and 41,170 price outliers in our primary and secondary datasets respectively.

In the end of our preprocessing the secondary dataset contained the 26 variables: price, artist, cardFinish, colorIdentity, colors, edhrecRank, ehrecSaltiness, gameAvailability, isReprint, language, layout, manaCost, manaValue, name, number, originalType, power, priceProvider, providerListing, rarity, setCode, supertypes, toughness, type, types, and uuid. The primary dataset contained one less variable (language) and consisted of 15,214 entries across 3,123 unique cards. The secondary dataset consisted of 228,637 entries across 50,208 unique cards. 

In card gaming resell markets, it is common to use and interpret Rarity and Collection(setCode) as empirical drivers for price. We want to use the relationship between the variables and clustering algorithms to understand if there are more correlations available prior to predicting prices. 

# Exploratory Data Analysis ([[Exploratory Data Analysis | EDA]])

This section aims to uncover patterns and insights within our preprocessed datasets. By thoroughly examining attributes, we will explore the relationships between these characteristics and their impacts on price. Through this analysis, we seek to provide valuable insights for our downstream models and assess the null hypothesis for each variable regarding its influence on price. The same analysis approach was taken for both the primary and secondary datasets. 

## Primary Dataset Exploration

To start evaluation we explored the correlations between variables to get a better understanding if our dataset contained multicollinearity and the impacts of that on regression models. After seeing the strong positive correlation between colorIdentity and colors at 0.97 and originalType and type at 0.75, we calculated a Variance Inflation Factor (VIF) to quantify and identify how redundant our variables are (Penn State Eberly College of Science 2018). The VIF values greater than 10 were colorIdentity at 20.31 and colors at 20.38, so we dropped the colorIdentity column from our primary dataset.

Next, we utilized the Test statistic to measure how far each point estimate deviates from the parameter value of price in relation to each variable. Additionally, we calculated the p-value to determine whether to reject or fail to reject the null hypothesis regarding the influence of each variable on price. Some key negative relationships to price are isReprint and edhrecSaltiness. Another note is that the priceProvider has some influence on price but not as much as the other relationships with price.

![[Screenshot 2024-10-22 at 09.21.04.png]]
_Correlation heatmap of the Primary dataset after feature mapping, but before dropping and transforming features._

## Secondary Dataset Exploration
For the secondary dataset we did a similar approach, beginning with coefficient correlations and VIF calculations. The VIF calculations resulted in a 26.92 for colorIdentity and a 26.93 for colors, so again we dropped colorIdentity from the dataset. The secondary results from statistical testing indicate that nearly all variables have extremely high T-scores and p-values of 0, suggesting a very strong statistical significance in their relations with price.

![[Screenshot 2024-10-22 at 09.22.41.png]]
_Shows T-score and P-values for each feature for the primary (Figure 2a, on the left) and secondary (Figure 2b, on the right) datasets._

To conclude our exploratory analysis, we examined the price distributions for each dataset. On the left, Figure 3a displays the histogram of prices from the primary datasets, where values are capped at $8 due to outlier removal, resulting in a maximum price of $7.76, a mean of $1.27 and a standard deviation of $1.68. On the right, Figure 3b shows the histogram for the second dataset, which was bounded at $5, with a maximum price of $4.73, a mean of $0.73, and a standard deviation of $1.00. This highlights the left-skewed nature of our price data, as the majority of MTG cards are priced below $1.

## Data Transformation
Our initial analysis revealed the significant positive skew in the price data, with a skewness value of roughly 1.88 and 2.08 for our primary and secondary datasets as seen above. This skewed distribution poses challenges for many statistical techniques that assume normality. We chose to adjust the data distribution through transforms to enable a mode robust statistical analysis and modeling process.To enhance the accuracy and reliability of subsequent insights into card prices and their relationships with other variables, explored multiple transformation methods to normalize the data distribution.
Our results for the primary dataset are explored below and those for the secondary dataset can be found in Figure 4. We implemented four transformation methods: square root, inverse, box-cox, and Yeo-Johnson. Among the applied methods, the Box-Cox and Yeo-Johnson transformations prove most effective, reducing the skewness from 1.88 to approximately 0.27. This substantial reduction brings the data distribution much closer to normality.
Histograms are provided in Figure 4 for each transformation, allowing for visual comparison of the distribution changes. These visualizations aid in identifying the most effective transformation method for the dataset.


![[Screenshot 2024-10-22 at 09.25.54.png]]
_Historgrams of each transformation performed. (a. Original Price feature, b. Square Root Price transformation, c. Inverse Price transformation, d. Box-Cox Price transformation, e. Yeo-Johnson price transformation)_

We extended the transformation process to encompass additional left skewed variables such as edhrecRank and edhrecSaltiness, ensuring a comprehensive approach to datanormalization across the entire dataset. By applying the Box-Cox transformation for each variable, we aimed to enhance the predictive accuracy of our model.

However, it’s important to acknowledge that this holistic approach introduces certain drawbacks. By adding a layer of complexity we can complicate the interpretation of model results as well as potentially obscure underlying relationships within the data. To mitigate these potential issues, we retained our non-transformed dataset for model evaluation, allowing for us to better understand the impacts of our transformations, which will be discussed in more detail with model results.

# Model Selection

We needed to select a model for price prediction, by taking into account the characteristics of our dataset. To gain additional insights beyond our statistical analysis, we decided to evaluate the dataset through clustering. Clustering is a fundamental technique in data analysis where similar data points are grouped together. It facilitates deeper insights into the underlying structure of the dataset.

## Self-Organizing Maps
In this section we will compare our prior manual insights and clustering analysis to determine our price prediction model. We chose Self-Organizing Maps (SOMs) as our clustering method based on a combination of research and our knowledge of the dataset. SOMs are particularly useful for visualizing high-dimensional data, allowing us to graphically evaluate the entire dataset in an interpretable way. By uncovering patterns and relationships, SOMs can significantly enhance our model predictions.

SOMs are a type of neural network that aid in interpretability through visualizations, often represented as heat maps. In these heat maps, each cell corresponds to a neuron in the SOM, with color–specifically red–indicating a high concentration of data points. Our overall SOM (see Figure 5) reveals two main clusters represented by the red squares on the bottom half, along with several less populated clusters scattered across the graph. This distribution suggests a complex interplay of multiple factors influencing the data.

![[Screenshot 2024-10-22 at 09.28.50.png]]

To further analyze these clusters, we overlapped the means of each data feature to illustrate the correlations and differences between neurons in our map. Our findings indicate that price is a significant driving factor within the SOM clusters, suggesting that we can effectively predict prices. These observations reinforce our previous statistical tests, such as p-values and statistical significance, strengthening our findings and allowing us to confidently move forward with the development of our price prediction model.

## ANOVA Test
In addition, we aimed to explore one dataset feature in detail, leading us to implement a one-way ANOVA test to show how artists affect price. To do this, we had our null hypothesis (H0) state that artists have no impact on prices, and the alternative hypothesis (HA) posited that they do. The analysis yielded an F-statistic of 5.04 and a p-value of 0.000000 for the primary dataset. Given that the p-value is significantly less than the conventional alpha level of 0.05, we reject the null hypothesis. This result indicates a statistically significant difference in average prices among cards from different artists, suggesting that some artists produce cards that are valued more highly in the market.

We believe that there are important impacts to consider. The results suggest that the artist associated with a card does influence its price, meaning that some artists may produce cards that are valued more highly in the market than others. For collectors and sellers, understanding these differences can inform buying and pricing strategies. We moved on to understand if, beyond the effect of artists to card prices, we would be able to predict card values based on our data.

From our statistical analysis we determined that the relationships in the data might exhibit both linear and non-linear patterns. Therefore we wanted to implement both a random forest and some regression models to capture different aspects of the data and compare their performance. This dual approach allows us to leverage the interpretability of linear regression while also benefiting from the flexibility and robustness of random forests.

# Model Analysis
Following the hypothesis testing, we used both Ordinary Least Squares (OLS) regression and polynomial linear regression to explore the relationships between card features and their market prices. The mapped datasets (both transformed and non-transformed) were divided into training and testing sets using an 80/20 split to validate model performance, and Polynomial features were generated to capture non-linear relationships between predictors and the target variable (price).

## Polynomial Regression

![[Screenshot 2024-10-22 at 09.30.20.png]]
_Polynomial regression models for both datasets as well as their transformed data._

The polynomial regression model model's performance was evaluated using two key metrics: Mean Squared Error (MSE) and R-squared (R2) values. The primary dataset model achieved an MSE of approximately 1.91, while the transformed primary dataset achieved approximately 0.00015. This significant reduction in MSE indicates a substantial increase in model accuracy when using the transformed data for predicting actual prices. However on the primary dataset, the R-squared values show a different picture. The non-transformed data had an R-squared value of 0.351, while the transformed data had a lower R-squared value of 0.97. This suggests that the non-transformed dataset explains a greater proportion of the variance in the prices compared to the transformed dataset.

The secondary dataset (recorded in Table 1) showed similar patterns and similar accuracy compared to the respective transformed or non-transformed dataset. One notable difference can be seen in Figure 6d, where the scattered blue points drop drastically around $0.28. We think that the regression model may be overfitting and underfitting in certain regions (particually the higher price values). At the upper end, there is a noticeable deviation where the predictions overestimate the actual values for some points and underestimate other sharply, creating the non-smooth pattern. 

Ultimately, these models are constrained by our original dataset choices, the feature mapping performed, and the data cleaning steps we outlined, including the removal of outliers so they were not designed to perform well on outliers.

## Ordinary Least Squares (OLS) Regression

The OLS model trained on the primary dataset achieved an R-squared value of approximately 0.498, indicating that about 49.8% of the variability in card prices can be explained by the independent variables included in the model. The adjusted R-squared was 0.497, suggesting that the model's explanatory power remains consistent even after accounting for additional predictors. The F-statistic was 654.9, with a p-value of 0.00, indicating that at least one predictor variable significantly contributes to explaining price variability. This reinforces the overall validity of the model.

When analyzing the results of the individual variables, we can see a clear relationship with multiple attributes of the model, with a few variables being really important:

Significant Variables:
- EDHREC Rank (-4.789e-05): Shows a negative relationship with price, indicating that as a card's rank increases (becomes less popular), its price tends to decrease.
- EDHREC Saltiness (0.8681): Positively correlated with price, suggesting that cards perceived as more powerful or "salty" are valued higher.
- Mana Value (0.0621): Indicates that higher mana value slightly increases card prices.
- Rarity (-0.4133): A slight negative coefficient suggests that as rarity increases, prices tend to decrease, which might be counterintuitive as we would expect to see that rarer cards tend to be more valuable. For this interpretation, we think it is prudent to remind the audience that this is from one days worth of price data. Meaning on this specific day there could have been more market saturation or less demand for rare cards.
- Game Availability (1.389): Strong positive impact on price, indicating that cards available in more game formats are valued higher.

The OLS model trained on the transformed primary dataset achieved an R-squared value of approximately 0.975, indicating that about 97.5% of the variability in card prices can be explained by the independent variables included in the model. The F-statistic was 2.7e4, with a p-value of 0.00, indicating similarly to the previous dataset that at least one predictor variable significantly contributes to explaining price variability, reinforcing the overall validity of our second OLS model. For further numerical representations of the the specific variables and details of the secondary dataset results, consult Appendex 8.

## Random Forest Regression

We moved forward to utilize the Random Forest Regression algorithm to understand if we can achieve a better prediction result using the same model definition. For this analysis we will focus in on the primary dataset as well as the transformed primary dataset.

![[Screenshot 2024-10-22 at 09.32.25.png]]


The random forest regression had the best performance on both the transformed and non-transfromed datasets, when analyzing the MSE and R-squared values. For the graphs in Figure 7a, the primary dataset, the MSE was 0.775 and R2 was 0.741. The transformed dataset showed even better results at an MSE of 8.18e-10 and an R2 of 0.999. This collaboratesn our previous model results, showing that the data transforms of the significantly left-skewed data results in almost perfect random forest predictions. This suggests that the transformation applied made it easier for the model to predict with higher accuracy.
 
 Another useful feature of random forests is the feature importance values, which we show in Appendix 9. It is important to note that both the primary and secondary datasets had differing feature importance rankings, as well as differences between the transformed and non-transformed data variations. However, across the board, we observed that edhrecRank (a card's popularity in EDH/Commander), edhrecSaltiness (indicating community sentiment about a card’s power level or fairness), and manaCost (especially in the secondary dataset) consistently had high feature importance values as strong predictors of price.

The importance of edhrecRank and edhrecSaltiness highlighted the significant impact of community perception and card popularity on pricing, and how this information is important to the perceived value of a card. The high importance of the artist feature, can also be connected to the set published, suggests that certain artists' works command higher prices, which could be valuable information for collectors. Game-related features (manaValue, gameAvailability) have importance, indicating that a card's utility in gameplay does influence its price, but perhaps not as much as its community perception values.

## Model Comparisons

Table 1
_Shows model Mean squared error and r-squared values for each regression model and each dataset both transformed and non-transformed._

| Dataset<br>                         | Polynomial Regression<br>        | OLS Regression<br>                   | Random Forest Regression<br>  |
| ----------------------------------- | -------------------------------- | ------------------------------------ | ----------------------------- |
| Primary Dataset<br>                 | MSE ~= 1.91<br>R2 ~= 0.35        | MSE ~= 2.24<br>R2 ~= 0.50            | MSE ~= 0.77<br>R2 ~= 0.74     |
| Primary Dataset<br>                 | MSE ~= 0.00015<br>               | MSE ~= 0.0011<br>                    | MSE ~= 8.18e-10               |
| (Transformed)<br>                   | R2 ~= 0.98                       | R2 ~= 0.98                           | R2 ~= 0.99                    |
| Secondary Dataset<br>               | MSE ~= 0.67<br>R2 ~= 0.33<br>    | MSE ~= 0.79<br>R2 ~= 0.49<br>        | MSE ~= 0.34<br>R2 ~= 0.66<br> |
| Secondary Dataset (Transformed)<br> | MSE ~= 0.00022<br>R2 ~= 0.94<br> | MSE ~= 0.00097<br>R2 ~= 0.96<br><br> | MSE ~= 1.44e-13<br>R2 ~= 0.99 |

Our radom forest regression consistently outperformed the other models, especially after data transformations, achieving near-perfect R2 values and minimal MSE. The polynomial regression and OLS regression show similar notable improvements after data transformation but are slightly less accurate. Though our model experimentation process we showed that regression modeling assumes a normal distribution of errors, since our price data was heavily left-skewed the non-transformed models did not perform as well.

The OLS regression provided a stronger explanatory framework for understanding price variability, as evidenced by its higher R-squared value and more significant coefficients.
While polynomial regression allows for capturing nonlinear relationships, it did not outperform OLS, suggesting that the relationships between features and price may be simpler than expected, with a largely linear structure driving the variability in prices. The random forest’s ability to capture complex interactions and non-linearities between features proved to be the most effective. The model’s flexibility in handling both linear and non-linear relationships likely explains its superior performance, especially after the data transformations. To build on the success of the random forest regression, we should consider further data exploration of additional features not selecting in out datasets.


# Conclusions
We started this article expecting to find possible attributes that could influence the prediction of prices, assuming that no external factors would be included in the dataset beyond the Card Attributes and Price information from a single market day. As we explore the data, we encounter multiple correlations between Price and Card Attributes that can point us to a generalization model for predicting price with an approximation of 55%. 

Our analysis can be useful when making a decision about trading multiple cards, and can influence buyers and sellers on better pricing strategy. 
The positive relationship between EDHREC Saltiness and price suggests that cards perceived as more desirable or playable are valued higher by collectors and players.

Rarity: Rarity is often influencing perceived value.We analyzed its impact on prices using regression models. The OLS results indicated a negative coefficient for rarity, suggesting that rarer cards might not always command higher prices due to market dynamics.

Price Provider: We analyze their influence on pricing trends. The OLS results indicated that certain providers were associated with lower average prices.
Finishes: The finish of a card (e.g., foil vs. non-foil) . The analysis revealed a positive coefficient for finishes, indicating that cards with special finishes tend to be priced higher on average.

Artist: With  artist name encoded, we performed an ANOVA test to determine whether different artists significantly impacted card prices. The results showed substantial variance in average prices across different artists, allowing us to conclude that some artists produce more highly valued cards.



# References

Agresti, A., & Kateri, M. (2021). Foundations of Statistics for Data Scientists: With R and Python (1st edition). Chapman and Hall/CRC.

Grand Prix (2024, October 3). MTG Wiki. https://mtg.fandom.com/wiki/Grand_Prix

Penn State Eberly College of Science. (2018). Variance inflation factor (VIF). Penn State University. https://online.stat.psu.edu/stat462/node/180/

Schmidt, Gregory (February 16, 2023). "Magic: The Gathering Becomes a Billion-Dollar Brand for Toymaker Hasbro". The New York Times. Archived from the original on January 17, 2024. Retrieved January 17, 2024.
