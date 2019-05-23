# Relationship between book's color and rating

## Abstract
How the color of a book's cover could influence the rating ? This is the main question we will try to answer by investigating the Amazon Customer Reviews dataset and the Amazon Book Cover dataset. We are looking for a relationship between the image and the corresponding review. If such relation is significant, we will discuss how and why. Also we want to see the evolution along time, both on the cover image, and on the reviews themselves. That means, are there significant color changes on book covers from 1995 to 2015 ? Is the ratings average increasing or decreasing from 1995 to 2015 ?

## Research questions

- What is the relationship between book cover and ratings ?
- How has book covers evolved over time (from 1995 to 2015) ?
- How has reviews evolved over time (from 1995 to 2015) ?

## Dataset

Two different dataset concerning books will be used. The first one is the Amazon Customer Reviews dataset which contains a huge amount of reviews from 1995 to 2015. This dataset is available from the ic cluster. The second one is an Amazon Book Cover dataset found on github : https://github.com/uchidalab/book-dataset/blob/master/Task2/book32-listing.csv.

Columns details for the Amazon Customer Reviews dataset
- marketplace: 2 letter country code of the marketplace where the review was written.
- customer_id: Random identifier that can be used to aggregate reviews written by a single author.
- review_id: The unique ID of the review.
- product_id: The unique Product ID the review pertains to. In the multilingual dataset the reviews for the same product in different countries can be grouped by the same product_id.
- product_parent: Random identifier that can be used to aggregate reviews for the same product.
- product_title: Title of the product.
- product_category: Broad product category that can be used to group reviews.
- star_rating: The 1-5 star rating of the review.
- helpful_votes: Number of helpful votes.
- total_votes: Number of total votes the review received.
- vine: Review was written as part of the Vine program.
- verified_purchase: The review is on a verified purchase.
- review_headline: The title of the review.
- review_body: The review text.
- review_date: The date the review was written.

Columns details for the Amazon Book Cover dataset
- [AMAZON INDEX (ASIN)] : The amazon index of the book. It corresponds to the column 'product_id' of the Review dataset.
- [FILENAME] : The name of the image
- [IMAGE URL] : The url where the image is hosted
- [TITLE] : The title of the book
- [AUTHOR] : The author of the book
- [CATEGORY ID] : The category id of the book
- [CATEGORY] : The category of the book

## Process

First, we merge the two dataset. The first one is composed of three tsv files compressed with gzip. The second one is a csv file of 50 Mb only. This small size is not a problem for the merge, because what we want is to have the corresponding image for each review. Since some reviews may corresponds to the same book, it's very likely that each line of the cover dataset will be used several times.

The Build_dataset notebook will load the two dataset and perform the merge. After the merge, we end up with a pandas dataframe of length 1'841'789. We save the corresponding csv as merge.csv (~1.53 Go).

That Data_analysis notebook will load the merge.csv file into a pandas dataframe, clean the dataframe, and perform some basic visualization on the different features to better understand the dataset.

At first glance, the research questions seem pretty simple to answer. The main trouble we have is concerning the ratings. Indeed, we cannot simply consider the 5-stars rating system to judge whether someone likes the book or not. This is because most of the reviews (71%) give the maximum rating (5 stars). Statistic are less meaningful with such a bias.

We have two possibilities to overcome this issue. The first one is to remove every reviewers whose reviews are low in terms of discriminative powers (i.e. they always give the same rating). The second one, more difficult, is to infer a star rating using only the text reviews.

Therefore, we will use text classification to find a model which can be used to predict the ratings of each review. We will split our dataset into a train set and a test set. Once the model have a good accuracy, we will set weights to both the prediction and the actual star rating, in order to compute an adjusted star rating. We will keep a floating point precision for this new score.

Once we have this new attribute, we will put it in relation with the book cover image to see if we can find a relationship. Also, we will see how this new score evolves over time.

## Contributors

[Mike Bardet] , [Diana Petrescu]