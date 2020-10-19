# Sentiment of Tweets Related to Covid-19 Policies

## Problem Statement

Our government’s response to COVID-19 has largely varied by region and state.  Some cities have enacted strictly enforced stay-at-home and mask policies while others have put out general guidelines.  Similarly, and perhaps consequently, opinions over COVID-19 and the policies that have followed have varied widely in the nation.  We hoped to visualize the similarities and differences between states and regions based on the overarching COVID-19 policies that have been enacted using publicly available social media data.

## Summary

With the rampant spread of the new Coronavirus in the United States, we see a subsequent and similarly rampant spread of information and opinions throughout our nation.  From state shelter-in-place policies being enacted as well as trending hashtags, we seek to understand the sentiments behind Covid-19 tweets that may shed light to how the nation feels about the new virus.

In order to see sentiment in any meaningful way, we hope to collect a large amount of tweets.  This could be done in several ways:  through open-source APIs, through websites that have already collected this data, or a combination of both.  We chose the latter.  Datasets from [Kaggle](https://www.kaggle.com/smid80/coronavirus-covid19-tweets) and Rabindra Lamsal via [IEEE Data Port](https://ieee-dataport.org/open-access/coronavirus-covid-19-tweets-dataset#files) were used along with the open-source API project, [GetOldTweets3](https://github.com/Mottl/GetOldTweets3). The first dataset had roughly 170,000 tweets between 3/29/2020 and 4/30/2020 while the IEEE Dataset had roughly 65,000 tweets from 3/19/2020 and 8/12/2020.  These geo-tagged tweets were limited to the U.S., cleaned by eliminating columns with significant null values, and filtered to delete uninterpretable place names where we saw fit.

The next data collection objective would be different characteristics of state government policies including shelter-in-place policy dates, school closing dates, emergency declarations, and travel bans.  KFF had one of the most comprehensive datasets for state Covid-19 policies. The policy information was mostly derived from KFF's [github](https://www.google.com) of raw data used in their website visualizations. Their visualizations can be viewed on KFF's [website](https://www.kff.org/policy-watch/stay-at-home-orders-to-fight-covid19/
) . 

The mask policy data was gathered from the organization [Masks4All](https://masks4all.co/what-states-require-masks/) , a volunteer organization tracking and promoting mask policies. 

Next, the lexicon and rules-based sentiment analysis tool, [VADER](https://github.com/cjhutto/vaderSentiment), came in handy once we've gathered our data.  We want to tie a rule-based, numerical score to each tweet so that we can aggregate our data in numerous ways.  That is what VADER delivers.  It uses a large dictionary specifically trained on social media posts and assigns a valence score to each word.  Then using a rules-based method, it gives you a compound (overall) score, and a positive, neutral, and negative score that signifies the proportion of text.

## Results

Our results can be seen below:

![Negative State Sentiment Analysis w/ Stay-at-Home Policies graphs](/Images/state_negativity_trends.png?raw=true)

The above graphs show a rolling 7-day average of negative sentiment scores in four states.  The gray band represents when the stay-at-home order was enacted and when it was lifted.

## Conclusions

The sentiment for tweets skews in the positive direction with lowest average compound scores being around .09 nationally.

National Sentiment Analysis:

![national sentiment graph](/Images/national_sentiment.png?raw=true)

Many tweets from the Kaggle dataset heavily “smoothes” the sentiment data since there were a huge number of tweets concentrated in the month of April

![discrepancies in available data graph](/Images/sentiment_by_data_availability.png?raw=true)

There was no big negative trend in sentiment based on shelter-in-place policies in target cities.

![Little Link Between Key Policies and Sentiment graph](/Images/orders_vs_sentiment.png?raw=true)

# Next Steps

Future data collection is a must, but social media collection of other dates would have been viable only if we had more time. This could include the expansion of keyword lists beyond those used by the provided datasets, though the most straightforward approach would be to process the larger dataset collected by Rabindra Lamsal (also via [IEEE](https://ieee-dataport.org/open-access/coronavirus-covid-19-tweets-dataset#files)). This dataset includes nearly all tweets meeting the criteria regardless of geotagging, which would likely mean a significant number of tweets without any associated location data. However, the dataset includes more than 416 million tweets as of August 14, meaning even a modest percentage of additional tweets identifiable as originating in the US could mean a significant boost to the dataset.

The next most important step would likely be the development of a sentiment baseline for twitter posts, allowing for hypothesis testing on whether COVID-related tweets are significantly more negative or positive. Additional models could also be developed to help identify which policies, if any, resulted in the largest sentiment impacts, as well as to draw correlations to death and new case rates.

## Files

| File/Folder | Description |
|-|-|
| `combined_data_cleaning.ipynb` | Notebook that looks through all files in Data folder and appends any new data to combined data file |
| `county_assignment.ipynb` | Notebook that looks through all files in Data folder and appends any new data to combined data file |
| `simplified_hydration_prep.ipynb` | Notebook that removes extraneous columns from tweet ID files |
| `unspecified_location_cleaning.ipynb` | Notebook that looks through all files in Data folder and appends any new data to combined data file |
| `eda_vader.ipynb` | Notebook that creates sentiment for every tweet |
| `time_series.ipynb` | MatPlotLib line plots for sentiment scores and policy dates |
| `state_policies.ipynb` | Notebook investigating state policies |
| `validating_city_names.ipynb` | Notebook checking database city names against external reference |
| `NY_city_policies.ipynb` | Notebook investigating NYC policies |
| Data | Data folder that holds `.csv` data files. Excluded from repository due to size |
| Reference Files | Folder of sources and presentation materials. Duplicates reference files from Data folder |

## Data Dictionary of `all_sentiment.csv`
| Columns | Description |
|-|-|
| `created_at` | date and time of tweet creation |
| `favorite_count` | count of people who favorited the tweet |
| `status_id` | id of tweet |
| `lang` | language of tweet |
| `place_full_name` | most city and state; occasionally state and country |
| `retweet_count` | count of people who retweeted originally tweet |
| `text` | text of tweet |
| `account_created_at` | data and time of account creation |
| `screen_name` | tweet author's username |
| `followers_count` | count of people who follow tweet author |
| `friends_count` | count of people who are followed by tweet author |
| `verified` | whether or not the author's identity has been verified by Twitter |
| `city` | geo-tagged city of tweet account |
| `state` | geo-tagged state of tweet account |
| `is_reply` | whether or not the tweet was in reply to another tweet (Boolean) |
| `is_retweet` | whether or not the tweet was retweeted (Boolean) |
| `country_code` | country of tweet account |
| `place_type` | city, admin, or unspecified |
| `neg` | negative VADER sentiment score |
| `neu` | neutral VADER sentiment score |
| `pos` | postive VADER sentiment score |
| `compound` | compound VADER sentiment score |
| `county` | inferred county of tweet account |

## Data Dictionary of `state_policies_new.csv`

| Columns | Description |
|-|-|
| `state` | US state abbreviations |
| `state_full` | full spelling of US states |
| `date_stay_home` | day a shelter in place order was issued |
| `date_travel_quarentine` | day the state began quarentining travelers |
| `date_nonessential_close` | day states closed nonessential business |
| `date_large_gather_ban` | day states began limiting large gatherings |
| `date_school_close` | day states ordered schools closed |
| `date_rest_limit` | date restaurant limits were imposed |
| `primary_delay` | status of primary election changed due to Covid-19 |
| `date_emergency_dec` | day governors issued states of emergency |
| `date_end_stay_home	` | day shelter in place orders were ended or expired |
| `date_end_travel_quarantine` | day travel quarantines were ended |
| `date_open_nonessential	` | day nonessential businesses were allowed to reopen |
| `date_end_large_gather_ban` | day bans on large gatherings were lifted or significantly relaxed |
| `date_end_school_close` | day in-person schooling resumed |
| `date_end_rest_limit` | day restaurant limits were ended or significantly relaxed |
| `date_mask_required	` | day masks were required in public |
| `mask_required` | type of mask requirements issued |
| `shelter_issued` | if a shelter in place order was ever issued |
| `travel_quarentine_issued` | if the state ever quarentined travelers |
| `nonessential_close_order` | if a state closed non-essential businesses |
| `large_gather_ban_issued` | if a state banned large gatherings |
| `rest_limit_issued` | if a state placed limits on restaurant service |
| `emergency_declaration` | if a state issued a state of emergency |
| `end_stay_home` | if a state ended shelter in place orders by date of publication |
| `end_travel_quarantine` | if a state ended travel quarantines |
| `open_nonessential` | if a state allowed nonessnetial businesses to reopen |
| `end_large_gather_ban` | if end_large_gather_ban |
| `schools_open_in_person` | if a state reopened in-person classes |
| `end_rest_limit` | if a state ended or relaxed restaurant service limits |