# Amazon Vine Analysis

## Overview of the Analysis

In this analysis, we are looking at Amazon reviews written by members of the paid Amazon Vine program. The Amazon Vine program is a service that allows manufacturers and publishers to receive reviews for their products. Companies like SellBy, a fictional company for the purpose of this module, pay a small fee to Amazon and provide products to Amazon Vine members, who are then required to publish a review. 

Using a dataset of outdoor products on Amazon, we want to explore any positive or negative bias in the Amazon Vine reviews. To accomplish this goal, we will perform an ETL process to extract the dataset, transform the data, connect to an AWS RDS instance. Then we will use PySpark to determine if there is any bias from Vine members.

### Analysis Resources
* **Data Sources:** [Amazon Outdoor Product Reviews](https://s3.amazonaws.com/amazon-reviews-pds/tsv/amazon_reviews_us_Outdoors_v1_00.tsv.gz)
* **Software:** Google Collab Notebook, Amazon Web Services, PostgreSQL 11

## Results of the Analysis

### Vine and non-Vine Reviews
[Amazon Outdoor Product Reviews] dataset contained over 2 million total reviews. Because we wanted to determine bias of the vine reviews, we need to filter the reviews to find ones that users thought were helpful. We decided 20 or more total votes would give us a sufficient size dataset to proceed with our bias analysis. To narrow it down further, we retrieved all the rows in the dataframe where the number of helpful votes divided by total votes is equal to or greater than 50%. We named this dataframe: ```vine_helpful_votes_df```

#### First Filtered Dataframe
<img src="https://github.com/dwwatson1/Amazon_Vine_Analysis/blob/main/images/Filter_1.PNG" width="600" >

#### Second Filtered Dataframe
<img src="https://github.com/dwwatson1/Amazon_Vine_Analysis/blob/main/images/Filter_2.PNG" width="600" >

Using ```vine_helpful_votes_df```, we could filter it once more to find out how many of those reviews were part of the Amazon Vine Program (paid). ```vine_paid_helpful_df = vine_helpful_votes_df.filter("vine == 'Y'")```. Using this filtered dataframe, we determined that there were **107** paid reviews for outdoor products out of **39,976** total.

#### Vine (Paid) Filtered Dataframe
<img src="https://github.com/dwwatson1/Amazon_Vine_Analysis/blob/main/images/Filter_3.PNG" width="600" >

#### Total Helpful Reviews
<img src="https://github.com/dwwatson1/Amazon_Vine_Analysis/blob/main/images/Count_1.PNG" width="600"> 

#### Vine Reviews Count
<img src="https://github.com/dwwatson1/Amazon_Vine_Analysis/blob/main/images/Count_2.PNG" width="600"> 

#### Non-Vine (Unpaid) Filtered Dataframe

For non-Vine Program reviews (unpaid), we simply changed the filter from Y to N: ```vine_unpaid_helpful_df = vine_helpful_votes_df.filter("vine == 'N'")```. Using this filtered dataframe, we found that there were **39,869** unpaid reviews for outdoor products out of **39,976** total.

<img src="https://github.com/dwwatson1/Amazon_Vine_Analysis/blob/main/images/Filter_4.PNG" width="600" >

#### Non-Vine Reviews Count
<img src="https://github.com/dwwatson1/Amazon_Vine_Analysis/blob/main/images/Count_3.PNG" width="600"> 

### 5-Star Ratings of Vine and non-Vine Reviews

To further explore bias in reviews, we wanted if Vine reviewers tended to give 5-star ratings more often than non-Vine. To begin, we needed to find the 5-star ratings for each type of review, we filtered the ```vine_helpful_votes_df``` by the star_rating column ```total_five_star_reviews_df = vine_helpful_votes_df.filter("star_rating == 5")```. Using the new ```total_five_star_reviews_df``` dataframe, we filtered it again by the vine column, Y and N, similar to what we did in the last section.

There were a total of **21,061** 5-star reviews, of those, **56** were paid and **21,005** were unpaid.

#### Total 5-Star Reviews Count
<img src="https://github.com/dwwatson1/Amazon_Vine_Analysis/blob/main/images/Count_4.PNG" width="600"> 

#### Vine 5-Star Reviews Count
<img src="https://github.com/dwwatson1/Amazon_Vine_Analysis/blob/main/images/Count_5.PNG" width="600"> 

#### Non-Vine 5-Star Reviews Count
<img src="https://github.com/dwwatson1/Amazon_Vine_Analysis/blob/main/images/Count_6.PNG" width="600"> 

### Percent 5-Star Ratings of Vine and non-Vine Reviews

To complete our bias analysis, we needed to see what percent of the total Vine and non-Vine reviews were 5-star reviews and compare. Using the variables we calculated in previous steps, we determined the following percentages:

#### % 5-Star Vine Reviews
56 out of 107 Vine Reviews = **52.34%**

<img src="https://github.com/dwwatson1/Amazon_Vine_Analysis/blob/main/images/Percent_1.PNG" width="600"> 

#### % 5-Star non-Vine Reviews
21,005 out of 39,869 Vine Reviews = **52.69%**

<img src="https://github.com/dwwatson1/Amazon_Vine_Analysis/blob/main/images/Percent_2.PNG" width="600"> 

Given these results, Vine reviews were no more and less likely to give outdoor products a 5-star review. The results also show that there does not appear to be any bias, positive or negative, in the Vine review program for outdoor products.

### Additional Analysis

To further this conclusion, we wanted to find out just how small of sample the 5-star Vine reviews were among all 5-star reviews.

#### % 5-Star Vine Reviews of Total
56 out of 21,061 Vine Reviews = **0.27%**

<img src="https://github.com/dwwatson1/Amazon_Vine_Analysis/blob/main/images/Percent_3.PNG" width="600"> 

#### % 5-Star Vine Reviews of Total
21,005 out of 21,061 Vine Reviews = **99.73%**

<img src="https://github.com/dwwatson1/Amazon_Vine_Analysis/blob/main/images/Percent_4.PNG" width="600"> 

Because of 5-star Vine reviews represent such a small portion **0.27%** of all 5-star reviews, they are unlikely to have much positivity bias for reviews in dataset. 
