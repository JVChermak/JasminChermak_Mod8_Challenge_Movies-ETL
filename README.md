# Movies-ETL
## Project Overview
Amazing Prime requires three sets of data, using the ETL methods, in order to perform a hackathon.

## Resources
- Data source: wikipedia-movies.json, movies_metadata.csv, ratings.csv
- Software: Anaconda 4.7.12, Pandas, Jupyter Notebook, pgAdmin4

## Challenge Overview
Britta needs to create an automated pipeline for new data using the following steps:
- Create a function that takes in three arguments:
  * wikipedia data
  * Kaggle metadata
  * MovieLens rating data (from Kaggle)
- Use code from Jupyter Notebook so that the function performs all of the transformation steps.
- Add load steps, removing existing data from SQL but leaving the empty tables

## Challenge Assumptions
In order to automate the ETL pipeline, several assumptions are made about any future data that may be scraped.
1. The CSV file may be large
  - The ratings.csv file was very large in the initial ETL and required the use of low_memory=False. To help with this in the future, both ratings and movies_metadata use this condition when importing the files.
2. There are columns with similar names and the same data.
  - There were several columns that had similar names that were combined in the initial cleaning. These same columns are combined, assuming that there are not more that could be combined.
3. Some columns may have mostly null values.
  - Due to the cleaning, some columns had large quantities of null values. During the automated cleaning, we will still only be keeping columns with less than 90% null values.
4. Several data types were automatically assigned incorrectly.
  - A number of columns were found in the initial cleaning to have incorrect data types assigned. These columns are reassigned to the correct data type so that future manipulations are possible.
5. For the competing data in the Wiki and MovieLens data sets, the competing data will follow the same pattern as in the initial cleaning.

Competing data:
Wiki                     MovieLens                  Resolution
-----------------------------------------------------------------------------
title_wiki               title_kaggle               Kaggle more consistent, Drop Wiki
running_time             runtime                    Keep Kaggle, fill in zeros with Wiki
budget_wiki              budget_kaggle              Keep Kaggle, fill in zeros with Wiki
box_office               revenue                    Keep Kaggle, fill in zeros with Wiki
release_date_wiki        release_date_kaggle        Kaggle complete, Drop Wiki
Language                 original_language          Kaggle more consistent, Drop Wiki
Production company(s)    production_companies       Kaggle more consistent, Drop Wiki

  - After looking at several columns that had competing data, decisions were made to remove the Wiki data for titles, release date, language, and production company.
  - The wiki data was used to fill in missing pieces in the Kaggle data for run time, budget and revenue.
  - I am assuming that future data will follow similar patterns of Kaggle having more consistency and wiki being able to fill in the gaps.
6. Merging the movies_df and rating_counts with a left join will involve missing values because not all movies are rated.  
