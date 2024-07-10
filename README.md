# Project 3 - Reddit NLP

The aim of this project was to build a classification model/s that could accuractely predict which posts were from which subreddit, and then compare the two to see which was preferred. To do this, we also needed to scrape Reddit with use of an API and PRAW. After this, we would use natural language processing to treat the raw text as data to use in building our models.

We would first look through the posts, through some NLP, to see if there were any identifiers/key points to note that may affect our modeling. Cleaning is done in this step as well, as to preprocess the text, we would have to transform it into data we can parse through methods and processes.

Having made our models, we compare the two to see which performed better, and outline anything we could have done to improve, or note anything we think our project may have lacked.

### Data

As opposed to other datasets, since we are using text, there isn't really a data frame to cite, as we are essentially creating our own dataframe from the multitude of posts we scrape from Reddit via PRAW and API.

So our final data we use, would actually just be a dataframe of texts, posing as posts, and already classified to their respective subreddits.

## Executive Summary
 - First we had to collect our data, through use of PRAW and Reddit API. We use praw as reddit has a scraping limit of 100 per request, so through pagination, we were able to pull 1000 posts in one seemless go, as the PRAW would make multiple requests, then combine them into one dataset to use.
 - We used an .env file to hide our confidential keys and passwords, then loaded them using os. Then using praw, we accessed our Reddit API and loaded up our posts from the subreddits.
 - A problem we encountered was a max scraping limit, which was also compounded by a UI limit, of 1000 total posts, and this project was to collect at least 1000. Simply scraping through different categorisation of posts would allow us to join them and remove duplicates, as with a big subreddit, they are dealing with tons of posts on average over a short span of time.
 - Whilst scraping, took this from the project notes given by GA, adding a UTC timer to our scraping allowed us to see whether there was overlap with our pulls, potentially reducing the amount of duplicates, which we would then take care of later anyways, but was an easy way to deal with. Although, simply removing duplicates by title, but the UTC timer is more for the pagination method I talk about earlier, which helps with identifying where to start from for the API. So does not inherently remove duplicates, but potentially reduces the amount of them to begin with.
 - We cleaned the data, by removing duplicates by title, which makes sense, and then filling in the self-text/posts column with the title, as I noticed that people formatted their posts in this manner, using the title as the post itself, but then putting images in the post body.
 - As we said earlier, we put scraped 2 different categorisation of posts from each subreddit, to exceed the 1000 post limit. We simply concatenated them vertically then removed unnecessary columns for ease.
 - Here we get to a split, we used CountVectorizing for the individual EDA of subreddits, for we treated all the posts as relevant, but then Tfidf Vectorized for the total dataframe(both subreddits concatenated together) for indirectly, it would help with feature engineering for the model.
 - During EDA of the individual subreddits, we identified trends in key words, as well as the spread of those key words. Which proved to show some interesting connection to our model later.
 - Sentiment Analysed the posts of each subreddit to mark how "positive" each one was, with the given assumption that people love pizza more, but it turns out sourdough users are more likely to use positive language than pizza users, perhaps indicating their love for sourdough was greater and concentrated.
 - Creating our models was relatively simple, set up a pipeline and gridsearch so that we could preprocess and instantiate our models onto our data simultaneously. Added in some parameters to consider, but here we encountered an error, which adds to the point of simple language, that raising hte min-df, actually created not enough data points for our model to work from. Hence from here, I decided not to lemmatize/stem and raise min-df, for simple/informal language was what we were working with, and simplifying it further potentially lost us our nuances and data.
 - After our model creation, is when we started to explore how the model was built, given our EDA into the individual subreddits, and I assumed that our model was overfit/oversimple due to the informal language, and that ambiguity perhaps favoured one classification over another, due to it being slightly more varied/complicated than the other, sourdough vs pizza.
## Conclusion and Considerations
We found that the Logistic Regression model performed marginally better in almost all accounts, except for specificity. Beyond this, we found that the potentially the spread of, or lack of, key words worked against the pizza classifying class, and favoured ambiguous posts towards sourdough, or at least we theorise. Testing this with our list of ambiguous words, this was the case, but obviously this is too small a sample size to conclusively say, but the model definitely shows traits of this. 

I'd say, that overall, I achieved a decently performing model, which performs above 40% points better than our baseline accuracy, which was 50%. I can't complain about this score, but the metrics do show some overfitting, perhaps due to the simplicity of language in forum posts. Approaching this as a datascientist, we can always strive to aim for better predictive/classifying modeling and accuracy. There were definitely things to improve, which I shall outline.

First I'd like to outline that perhaps I approached this project too simply, for I just couldn't shake the feeling that I was finding things either too easily or even just settling for simple answers.

One thing I would note, is that our work around the 1000 post limit, is fine, but really only works on huge subreddits. Taking advantage of the different categories only work if there aren't many overlaps, which there weren't but that is because I chose big subreddits. When initially going through smaller ones, such as r/espresso and r/coffee, these workarounds wouldn't work; for there weren't enough posts which meant tons of overlap amongst the categories.

Whilst I did create a pretty accurate classification model, I feel as though I could have considered more classification models that may have worked better, such as random forest classification.

Regarding my point on ambiguous posts, and perhaps model favouring sourdough in these circumstances, whilst the graph of key word spread could indicate this, our test definitely had flaws inherently, for the list of words we used may all just be higher on the list of sourdough top words as opposed to pizza, which would also explain the result.

This adds on to the fact that perhaps posts on reddit wasn't the best data we could have used, for simple informal language, fed into a classification model, would create an overfit or oversimple model, which may work for this case scenario, but wouldn't be a great model in general, unadaptable, and specifically works for just the sourdough and pizza subreddits, which limits its use. Moving forward, making a multiclass classifier would perhaps be a worthy project to explore.