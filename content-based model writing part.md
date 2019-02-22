# Content-Based Recommendation System



## Main Structure

The content-based recommendation system in my mind, can be separated into three parts: feature extraction/preprocessing, profile setup (offline preference learning) and online prediction.

### Feature Extraction / Preprocessing

In this part has two subsections: preference range setup and separate several preference categories. 

* **Preference range set up**

  With the embedding of product description, I can compute the cosine distance between each product and save the top $k$ similar products using **k-NN**, say $k=50$. The range with 50 products centered by each product let's call it `preference range`. 

* **Preference categories set up**

  In order to set up a rough user preference, I can use **K-means clustering** to separate the products into, say $k = 10$ categories. Call these 10 categories as `initial preference category separation`. According to these categories and user ratings, I can generate the preference label for each user as training data in supervised learning. To be more specific, select the product description the user gave high ratings, or spent money on, then take the average of those product description embedding. By checking which categories that average point falls in, I can get an initial preference of this specific user.

### Profile Setup (Offline preference Learning)

Using the preprocessed data, we can use a supervised learning method to predict the initial preference for every user, even he or she doesn't have any historical record. Take   `age`, `gender`, `money_spent`, and `ratings` as features, and `category_preference` as labels, and train the model using random forest classifier. 

### Online prediction (recommendation)

Given a user's initial category preference, (it can come from computing historical user data or the predictive model), I can generate products recommendation by sampling from the preference range (it may only need to recommend 10 items, so I can sample 10 products from the whole preference range $n=50$, or I can just pick the top 5 items in this preference range). Or another way to generate the recommendation products is to $1-p$ proportion of products from the preferred categories and $p$ proportion of products from other categories, just like the $\varepsilon$-greedy algorithms in multi-armed bandit problem.

Collect user feedback on the recommendation, set the selected item as new preference range center, and generate recommendation using the same way as previous. 



## Pros and Cons

#### Pros

It is an online prediction, so it can adjust according to the user preference changes, which power the recommendation system.

It can solve the cold start problem. Even with know nothing about the user, we can still give a reasonable recommendation at the very beginning. 

It can also solve the problem for new items that have no historical ratings.

In the whole pipeline, it actually largely depends on Machine Learning domain, which means we have many off-the-shelf solutions. 

#### Cons

It tends to always give unsurprising results, and may focus too much in specific area.

Since it depends on the product's features like description, so the product's features needs to be very effective to give the recommendation a good start.

 It actually runs a single model for each user, so it may computational heavier than collaborative filtering model.

## Which one Emadri would prefer?

I think Emadri would prefer the collaborative filtering model. Since it actually been proved, empirically, more precise when generating recommendation.

Besides, if I understand correctly, Emadri will provide a recommendation on travel packaging list, which doesn't need to be so specific. If we also want to give products recommendation that users will potentially buy, then a content-based recommendation on possible shopping items might be more useful. 