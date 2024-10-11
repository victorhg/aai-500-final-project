aai-500-final-project
# USD AAI 500 final project - Group 3

Group members
- Victoria Dorn
- Victor Hugo Germano


Driver Folder with documents and colaboration files: https://drive.google.com/drive/folders/1YWShaGxksdq90fVHfnRuzuHFqex22Y60

# General Porpose of this project

## Introduction
Magic the Gathering is one of the most famous card games in the world. First published in 1993 by Wizards of the Coast, it has become a benchmark of the modern fantasy games, getting more than 40 million players around the world (Schmidt., 2023).

With more than 23,000 unique cards, the game is a phenomenon, having worldwide championships with cash prizes up to $ 10 million. For example, The Las Vegas Grand Prix championship receives close to 11,000 participants every edition (Grand Prix, 2024).

The game also has a thriving primary and secondary market for cards. The rarest card in the game, the Black Lotus, has been sold for $ 600,000 on an auction, with some cards receiving even higher expected value. 

We believe that analyzing card assets in relation to their general prices in the secondary market can offer valuable insights to collectors. The primary object of this project is to statistically analyze the price variable against various feature variables using methods learned in this course, and employ regression processes to predict prices. 

We decided to apply multiple regression and classification techniques to allow us to predict prices based on the categorical data from Cards, using Linear Regression techniques and Ordinary Least Square Regressions. We will then employ a Self-Organizing Map (SOM) to assist with clustering and visualization. Ultimately, using Random Forest Regression, we aim to compare some of the results obtained from the SOM with our own manual statistical evaluations and lay the groundwork for building trust in machine learning models for multidimensional datasets. Numerous fields can benefit from the application of simple machine learning algorithms such as SOMs.  

Given the maturity of the Magic: The Gathering card market, we have identified an open-source dataset that includes current (or relatively recent) market prices. This comprehensive dataset will be our ground truth, rather than aggregating data from multiple marketplaces. Subsequently, if time permits we would like to implement a predictive model for card price predictions using Card Attributes and Price Information from a single market day.

Secondary markets are influenced by many factors when pricing a card, many of which are external to the data used in the article, and that could be used in future research. Synergy between cards during gameplay, deck strategy in Tournaments, exclusive art and celebrity endorsement can affect price and will not be used in the analysis. 

## Quick Start
If you are looking to evaluate our statistical process this is the general order to follow: 
1. ensure that you have the cards.csv and cardPrices.csv - you can pull a new date for the Prices
2. run the `notebooks/preprocessing.ipynb` file
3. evaluate the primary and secondar data: `notebooks/eda_primary.ipynb` and `notebooks/eda_secondary.ipynb`
4. Explore our modeling

## Some Results

| Process                              | Result                              | Observation                                                               |
| ------------------------------------ | ----------------------------------- | ------------------------------------------------------------------------- |
| Polynomial Linear Regression | MSE: 2.367<br>R-squared: 0.32       |                                                                           |
| Ordinary Least Squares          | MSE: 2.618 <br>R-squared: 0.529<br> | EDH Saltiness and Game Availability with more influence in the result |
| Random Forest Regression         | MSE: 1.34<br>R-squared: 0.59        | EDH Rank and EDH Saltiness with more influence in the result              |




## Possible hypothesis
We are still analyzing the best hypothesis that would be feasible to our project, given time constraints 

- Card price prediction based on features and rarity
- Win rate estimations based on deck composition
- Card synergy compositions 
- Pay to win? influence of price on deck composition and win rate (possible hypothesis: win rate increases as you spend more money? Expensive decks win more?)

## Project Organization

- dataset: Contains all csv files used in this project
- notebooks: Contains all Jupyter notebooks

# General Assets

## Presentation Slides
https://docs.google.com/presentation/d/1JGvcu4lP4qdWhSUnkANtwXZufy2jb_T6_U-tCEs0KNg/edit?usp=drive_link

## Reference Data
Dataset: 
- Official dataset: https://mtgjson.com/getting-started/
- Data file (110Mb): https://mtgjson.com/api/v5/AllPrintingsCSVFiles.zip

All card dataset that includes all printings and variations of the Card Data Model, organized by Set’s code property. 

Cards information: 

- 97,145 card entries (25 variables in total)
- 5 main variables selected to analysis
    - rarity
    - edhrecRank
    - artist
    - power
    - color

Price information:

- 558,079 card price entries (8 variables in total)
- 3 main variables selected
    - price
    - gameAvailability
    - currency



## Reference Project Timeline

- Module 2 (by the end of Week 2): The course instructor will group students into teams of two to three members.
- Module 4 (by the end of Week 4): Each team will select and introduce a dataset. The team representative will submit the "Team Project Status Update Form."
- Module 7 (by the end of Week 7): Each team will submit the following deliverables for the course project in the final week:
- Technical Report: One PDF document with your final technical report. This should describe your preparation and analysis of the data, discuss the final model selection, and describe the statistics behind your final model selection.
- Team Presentation: One 8-10 minute video presentation by all team members. Do not exceed 10 minutes. Submit one mp4 file. This should be presented as a business presentation of your analysis to a non-technical audience. You will present your analysis and findings in a way that is understandable to any non-technical executive or business leader. This presentation should include one slide to showcase your collaborative efforts; you will create a presentation slide highlighting each team member's individual names and their contributions to the final project work and deliverables.
