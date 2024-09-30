aai-500-final-project
# USD AAI 500 final project - Group 3

Group members
- Victoria Dorn
- Victor Hugo Germano


Driver Folder with documents and colaboration files: https://drive.google.com/drive/folders/1YWShaGxksdq90fVHfnRuzuHFqex22Y60

# General Porpose of this project

## Introduction
Magic the Gathering is one of the most famous card games in the world. First published in 1993 by Wizards of the Coast, it has become a benchmark of the modern fantasy games, getting more than 40 million players around the world (Schmidt., 2023).

With more than 23,000 unique cards, the game is a phenomenon, having worldwide regional championships with cash prizes up to $ 10 million. For example, The Las Vegas Grand Prix championship receives close to 11,000 participants every edition.

The game also has a thriving primary and secondary market for cards. The rarest card in the game, the Black Lotus, has been sold for $ 600,000 on an auction, with some cards receiving even higher expected value.

We believe that there's a lot to learn from analyzing the card assets in relation to their general prices in the secondary market, and understand better how we can create models that allow us to predict price fluctuations and possibly understand how deck composition affects pricing. Our objective for this project will be to statistically analyze the price variable against other feature variables, so if time allows we can predict new card prices. 

Since the market for Magic cards is mature, we were able to find an open source dataset that includes current (or relatively current, depending on the last update made to the database) market prices. We will use that full dataset as ground truth instead of pulling our own data from various marketplaces. 

We plan to implement a Self-Organizing Map (SOM) to assist with clustering and visualization of our data and compare our own statistical methods with the inferences we gather from the SOM. This project can be applied to different fields for the validation of SOMs and their usefulness, as well as applying it to the Magic: The Gathering community of secondary markets.



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
