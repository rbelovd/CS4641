# Exploring the Relationship between Social Media Sentiment and Bitcoin
### Team Members
Trinity Davies, Joe Lazzarini, Robert Belovodskij, Nihar Shah

### Video Link: [Here](https://youtu.be/_HKf9gOEAuU)


### Team Member Responsibilities*
- Nihar: Research, design, and testing.
- Trinity: team/project logistics, keeping team on track for timeline, data collection
- Robert: Data Visualization, helping with data collection
- Joe: Research, architecture design

*Responsibilities may change in the future*

### Project Timeline
- Scrape data within the next two weeks to get instructor feedback and to see if we run into data sourcing issues, probably also a good time to evaluate if we should continue with our current topic (2 weeks from now)
- Pre-processing data within the next three weeks (3 weeks)
- Perform first supervised/unsupervised methods within the next month so that we can to be able to refine methods for better results (1 month)
- Overall team checkpoint to evaluate where we are in the project, discuss any results/issues we have had so far, midterm report (1 month+)
- Try to optimize method(s) and results, begin to polish and clean up result interpretation and prepare for final report  (6 weeks)
- Final report (8 weeks)


### Introduction

Bitcoin, with its decentralized structure and possibility for growth, has arguably been the biggest financial story of the last decade. However, with this potential there is a lot of volatility, much more than any stock in the stock market or national currency (Fan et al., 2020). With bitcoin being so volatile, is there any way to better understand what makes its price change so much? There has been a lot of research done trying to relate the sentiment of the news to the price of cryptocurrency, with some correlation being found (Karalevicius et al., 2018).
Generally, we are trying to determine the relationship between the price of bitcoin and Twitter sentiments, using the vadersentiment library and existing coin datasets.

### Problem Definition
With all of the hype surrounding cryptocurrencies within the past couple of years, there are too many variables to account for in determining which currencies may be the most profitable either long or short term. Especially when there have been specific individuals on social media platforms that are able to influence bitcoin prices via one simple tweet. We were looking to create a program that reviews online sentiment to determine whether or not people’s sentiment actually influences the price of the “hottest” cryptocurrency, bitcoin. However, as the project progressed, we realized that we could instead try to classify days based on both the sentiment and change in price. This will give us a better idea of the ‘bitcoin climate’ so to speak, and we can try to use our conclusions to better understand the relation between bitcoin and sentiment moving forward.




### Data Collection
We were able to find a couple of different datasets on Kaggle that contained tweets relating to Bitcoin. While manually scraping for tweets ourselves was an initial possibility, it turned out that scraping for tweets led to both messy and inconsistent data. Although Twitter does not have strong mechanisms to prevent data scraping, it proved to be a more complicated task than initially thought as without an appropriate mechanism to avoid being flagged and blocked from accessing Twitter. And because we were able to find and utilize good datasets that were already available, we simply decided to choose a highly rated dataset ("Bitcoin Tweets").
We were able to get bitcoin price and volume data very easily, and found a public dataset that we could use ("Bitcoin Data").
We had to clean the tweets (to create strings without special characters that could be analyzed by the vadersentiment library) to be able to have sentiment scores for any given day and correlate them to some level with the bitcoin price data. For the bitcoin data, we were able to easily extract both the volume and change in closing price of bitcoin, and had to do a bit of date adjustment to be able to get a change in price per day.
Following the initial run of our clustering, we determined that more data was necessary for a good result as the dataset we used resulted in only 79 days worth of data. We found a larger dataset which we combined with the initial one, which resulted in over 1000 days worth of data and led to better results for the clustering ("Bitcoin tweets - 16M tweets").


### Methods
After finding and collecting twitter sentiment data online, the data was parsed through to be cleaned. Once the data was cleaned, it was subjected to a VaderSentiment sentiment analysis. Once the sentiment of a certain tweet (positive, neutral, or negative) was recorded, an average sentiment value for each day was calculated and recorded to get the sentiment score for a given day. Several outliers were removed that had only one tweet for the day. We then used a dataset of the daily prices of bitcoin to determine the daily change in the closing price of bitcoin (The price at 11:59 PM).
The clean sentiment data was then plotted against bitcoin trading volume and change in closing price (net change) datasets to see if a relationship between sentiment and volume, sentiment and net change, or volume and net change can be seen. This way the following Figs. 1-3 were plotted.

Once we were able to plot the data, we decided to create a clustering model, specifically a Gaussian Mixture Model, to try and categorize the different types of interaction between sentiment and bitcoin value. After some testing, we decided on using 4 clusters. 

We also decided to apply linear regression to the dataset. To do this, we split our data into test and training datasets, with 80 percent of data in the training section. We then ran regression twice, with the daily change in closing price and the daily volume as labels for the data. R squared was calculated for both the test and training data, and was compared to determine if either model was overfitting.

![image](https://user-images.githubusercontent.com/20190532/145187586-e2ce96e9-ca91-4f35-9253-680b7223f083.png)*Fig. 1* ![image](https://user-images.githubusercontent.com/20190532/145187641-a3a3f631-8269-420e-92c5-7bb02c3f80f6.png)*Fig. 2*  ![image](https://user-images.githubusercontent.com/20190532/145187680-05eda908-8c17-4501-b832-b9191c2c1d96.png)*Fig. 3*


### Results and Discussion

When performing the clustering, we created models with between two and 20 clusters. The AIC, BIC, and Silhouette scores of each model were saved (Figures 4 and 5). The optimal number of clusters would have a high Silhouette score and low AIC and BIC. The graphs showed that four was the last number before the Silhouette score declined, and the first number where AIC and BIC stabilized at a low value, so we determined that the optimal number of clusters was four. The Silhouette score for the 4 cluster model was 0.43, so the clustering was a decent classification of our data.


![image](https://user-images.githubusercontent.com/20190532/145189017-09d4e0b3-4825-4085-b7c1-b9d115e451e0.png)*Fig. 4* ![image](https://user-images.githubusercontent.com/20190532/145189053-549ebfe1-c01c-401c-873b-a498bd7fedc1.png)*Fig. 5*


We initially intended to create a model which predicts the change in daily price from the sentiment and volume, so our first regression model used the daily change in price as the label for the data. However, we did not expect good results as there did not appear to be strong relationships in the data, or at least not linear ones (Figures 1 & 3). In accordance with our prediction, the model had an R squared of 0.02, so there was little relationship there (Figure 6). We then performed regression again using the volume as the label, since Figure 2 showed what appeared to be a linear relationship between sentiment and volume. In this case, we found a moderate-strength relationship as the R squared was 0.21 (Figure 7). Notably, the coefficient of the price change was two orders of magnitude smaller than the coefficient of the sentiment, which implies that sentiment was a better predictor of volume than the price change. Additionally, the results from the test and training sets had little difference, so the model did not overfit. We had also run ridge regression, but as expected, the R squared was slightly less than the standard regression model, so the standard model was better as there was no overfitting. Generally, we found that sentiment and change in price, especially sentiment, were fairly accurate predictors of volume - both price changes and sentiment were positively correlated with volume.

*Fig. 6*![image](https://user-images.githubusercontent.com/20190532/145189368-216d0b51-ff29-4698-87e9-647e3e70f95f.png) *Fig. 7*![image](https://user-images.githubusercontent.com/20190532/145189315-d0c425e3-7e58-4c9e-af61-5f3d86885653.png)

There were also several factors that could improve our models’ performance. Firstly, we would ideally scrape our own tweets from twitter, which would solve the problem of different days having different numbers of tweets. This would make the average sentiment of a given day more reliable, as it would be close to the true value. Additionally, an improvement could be made by better handling tweets in other languages. If possible, this would be using Vadersentiment in another language, but the tweets could otherwise be either removed or translated. Some of the tweets in our dataset were not in English, which reduces the accuracy of Vadersentiment since a major part of its algorithm is an English dictionary. If we could have incorporated non-English tweets into our data, we may have been able to get a bigger perspective and more data points to get a clearer look on the global scale of Bitcoin.

Throughout the process of this project, we came across several key points in time that changed the direction of the project from its first initial iteration. The first was that we were initially going to attempt to predict the prices of the 5 top cryptocurrencies using tweet sentiments. However, when we were looking for resources and datasets to use for the project, we discovered that apart from datasets of tweets about Bitcoin, we could not locate any datasets of tweets about other cryptocurrencies. Thus, we instead decided to only focus on Bitcoin instead of the five top cryptocurrencies.

Another key point was that while we thought that our inital total data values of 300 thousand tweets  would be enough to give us a good range of information, after cleaning the tweets and fully processing them, we saw that there were only approximately 200 days worth of data (Feb 2021 - Oct 2021). Combined with the fact that sentiment scoring tweets had the majority of them be neutral, we ended up losing quite a lot of time range and data points, which is why after looking at the information we have from the initial dataset, we supplemented our original dataset with additional datapoints from another dataset with more values and a longer range (16 million) by taking a randomized sample from the bigger dataset. 




### Conclusion
Our first goal of finding a correlation between average daily sentiment and Bitcoin price was not entirely successful. Despite the tweet cleaning that we did, there did not seem to be any notable relationship between price changes of Bitcoin and twitter sentiment. We did, however, find a relationship between twitter sentiment and the volume of bitcoin trades in a day, specifically, that volume tends to increase with positive twitter sentiment. Additionally, we found meaningful clusters which give a “snapshot” of a trading day for Bitcoin. While we did not find the conclusive link between price and sentiment that we were hoping for, we have discovered surprising relationships that could be the foundation for a more detailed analysis of the price/sentiment connection.


### References
[1] Fan, T., Su, Z., & Yin, L. (2020, October). *Economic fundamentals or investor perceptions? The role of uncertainty in predicting long-term cryptocurrency volatility.* Redirecting. Retrieved October 7, 2021, from https://doi.org/10.1016/j.irfa.2020.101566.

[2] Karalevicius, V., Degrande, N., & Weerdt, J. D. (2018, December 1). *Using sentiment analysis to predict interday bitcoin price movements*. The Journal of Risk Finance. Retrieved October 7, 2021, from https://doi.org/10.1108/JRF-06-2017-0092. 

[3] Stenqvist, E., Lönnö, J. (2017, June). *Predicting Bitcoin price fluctuation with Twitter sentiment analysis.* Retrieved October 6, 2021 from https://www.semanticscholar.org/paper/Predicting-Bitcoin-price-fluctuation-with-Twitter-Stenqvist-L%C3%B6nn%C3%B6/0954565aebae3590e6ef654fd03410c3bdd7d15a

[4] "Bitcoin Tweets,"  Kaggle. (2021, Nov.). https://www.kaggle.com/kaushiksuresh147/bitcoin-tweets.

[5] "Bitcoin tweets - 16M tweets", Kaggle. (2019, Nov.). https://www.kaggle.com/alaix14/bitcoin-tweets-20160101-to-20190329

[6] "Bitcoin Data", Kaggle. (2021, Nov.).  https://www.kaggle.com/varpit94/bitcoin-data-updated-till-26jun2021









