###### Run above in https://nbviewer.jupyter.org/github/danturti/Yelp_MythBuster/tree/master/ - for rendering jupyter notebook(.ipynb) if it fails to run
# Yelp_MythBuster

Yelp is a local area search service which publishes crowd-sourced reviews based on local businesses with a focus on restaurants.

The primary goal of this project is to apply machine learning and NLP modelling on Yelp user reviews from Thai restaurants in the city of San Francisco to predict and recommend the most popular Thai dishes in the city and popular restaurant specific dishes.

Before we even move into dish recommendation, I asked my peers who use the Yelp platform how they would pick a restaurant and a good dish there. Unanimously the approach was based of the “star” rating and the “number” of Yelp reviews of a business. Once at the business location they would ask the server what the special was.

The above was a good starting point as a “star” rating would be something we would gravitate towards and if a restaurant has a large “number” of reviews it must be reputable and popular right?

## Data Gathering/Collection
 
 - BeautifulSoup, Spiders (scrapy)
 
•	To do a baseline EDA and inferential analysis, BeautifulSoup was used to scrape the Restaurant Name, Rating, Price, Number of Reviews and Restaurant Type from the top 40 restaurants sorted by number of reviews. (Workbook1 dataset)

•	BeautifulSoup was used to collect all the reviews for the above respective restaurants as a string for analysis. (Workbook1 dataset)

•	Spiders was used to granularly collect information at a restaurant level and extract every single review for the modelling and NLP. Spiders was used as it is a multi-threaded process and the data was being collected was computationally expensive for Soup. (Workbook 2/ Workbook 3 dataset)


## Workbook 1(Restaurant Overview and Baseline Inferences)

-Packages used: pandas, numpy, matplotlib, seaborn, nltk

In this python workbook, baseline EDA and inferential analysis was performed on the features which include the Rating, Price, Number of Reviews. 

Histograms and distribution charts have been plotted for the above to see their individual spread among the restaurants

The reviews string is appended to the restaurant overview data set and sentiment analysis is performed on the reviews to add more dimensions to the data and the positive, negative and neutral sentiment is calculated for the all the restaurants.

The main inference is rating is positively correlated with the positive sentiment and negatively correlated with the negative sentiment. The Thai restaurant can reasonably represent the perception of a restaurant as is expressed in the reviews. However, the correlation between review count and rating is inconclusive. 

More data needs to be analysed and reviews need to be analysed individually against rating as opposed to the review count.


## Workbook2(Restaurant Review Analysis Modelling and NLP on tokens and topics)

-Packages used: pandas, numpy, matplotlib, seaborn, nltk, sklearn, gensim

In this python notebook, the dataset has individual reviews from each restaurant and the rating associated with each review. EDA involved in finding a strong linear relation of rating v/s features like price, positive sentiment of review, negative sentiment of review using NLTK from scatter plots. 

It can be inferred that the rating and price do not have a strong linear relation which debunks a misconception at least for Thai that an expensive business offering might have a much better product. Positive and Negative sentiments are good indicators of the reviews.

The first model deployed is a binary classification model which splits the classes into good rating (4 “star” and 5 “star”) restaurants and bad restaurants (3 “star” rating and below) as targets and uses the text corpus from the reviews as features. The text corpus is tokenized using the Count Vectorizer and TFIDF Vectorizer after text pre-processing and applied in a Logistic and Multinomial Naïve Bayes model. This is to objectify the ratings based on just the food, positive and negative sentiment words.

The Logistic Regression with TFIDF Vectorizer returns an accuracy score of .87 and a precision, recall score of .93 and .86 respectively. We are optimizing for False positives, so this baseline model is a good approach to look for food tokens and classify popular dishes.

The next approach is to perform unsupervised machine learning on the text corpus for word tokens to find food tokens for creating a dish data set.

The goal of word vector embedding models, or word vector models is to learn dense, numerical vector representations for each term in a corpus vocabulary. If the model is successful, the vectors it learns about each term should encode some information about the meaning or concept the term represents, and the relationship between it and other terms in the vocabulary. Word vector models are also fully unsupervised — they learn all these meanings and relationships solely by analysing the text of the corpus, without any advance knowledge provided.

The first approach is using Word2Vec to calculate cosine similarity of food related tokens. The tokens in the corpus are evaluated iteratively as uni-grams, bi-grams and tri-grams. The tri-grams evaluated give us the strongest cosine similarity as food menu items are generally a group of two to three words. Also, word algebra is performed on the tokens by adding and subtracting the vectors to draw contextual inferences for the same.

The second approach is to perform an LDA modelling on the same. In NLP applications, documents are represented a mixture of the individual tokens (words and phrases) they contain. There are two layers in this model — documents and tokens — and the size or dimensionality of the document vectors is the number of tokens in the corpus vocabulary.

Using LDA documents are represented as a mixture of a pre-defined number of topics, and the topics are represented as a mixture of the individual tokens in the vocabulary thereby reducing the dimensionality of the model.

LDA is fully unsupervised. The topics are "discovered" automatically from the data by trying to maximize the likelihood of observing the documents in your corpus, given the modelling assumptions. LDA uses a simplifying assumption known as the bag-of-words model. In the bag-of-words model, a document is represented by the counts of distinct terms that occur within it. The goal here is to do topic modelling to find as many food related tokens for analysis.

The LDA model is a little abstract for the dimensionality of the given data-set. However, critical food related tokens can be extracted from this.


## Workbook3

-Packages used: NLTK, sklearn, Textblob, pandas

In the final notebook, we work towards our final goal which is Review modelling to find the best Thai dishes in San Francisco.
A sample menu is created using topic modelling and reviewing menus from business listings for list of menu items.

Three scoring metrics are created for scoring dishes, 

•	Average Rating Score: All the reviews are iterated through and we look for menu items in the sample menu. The menu items are scored based on the user rating for the review and the average of the rating across the corpus is calculated as the food rating score.

•	Average Positive Sentiment Score: The same step is repeated with the main difference being the polarity score for the reviews is used to give the food items a score.

•	Granular Average Positive Sentiment Score: A review can have several sentences and a review can have multiple food dishes at the same time, the reviews are further broken down into individual sentiments and a score is calculated for each. This is a more accurate score for the food items.

All the above scoring metrics give very similar top dishes albeit some subtle differences. Across the city of San Francisco, it is inferred that Coconut Ice Cream is unanimously loved, roast duck is the favourite entree, Tom Ka is the best Soup, Panang and Pumpkin curry are the favourite curries, Wings are the best appetizer, pad kee mao is the best noodle dish and chicken is the protein of choice.

A linear regression model is applied on the same to see if the beta coefficients are consistent with the above dishes. 

Finally, a restaurant recommender function is created based of a master menu which contains items unique to certain restaurant offerings and the best dishes are scored based of the Granular Average Positive Sentiment Score for surprisingly accurate results.

A restaurant XXXX had the top dishes as Volcanic Beef, Tuna Tower, Sticky Rice and Grilled Salmon whereas another restaurant YYYY had more traditional top dishes like Pad See Ew, Imperial Rolls, Samosas and Iced Tea.

