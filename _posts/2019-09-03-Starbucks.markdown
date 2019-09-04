---
layout: post
title:  "Starbuck Capstone (Data Science)"
date:   2019-09-03 19:21:21 +0300
categories: DS
author: "MQ"
---



{:refdef: style="text-align: center;"}
![Cover](/assets/s0.jpg)
{: refdef}

### Notebook link

**Note:** The notebook is availble on my github [here](https://github.com/Malanquri/starbucks)
### Project Overview

In this project we utilize simulated data from Starbucks App to get insights on custome's behaivor with offers.
The data consists of three parts 
1. portfolio - which includes information about each offer.
2. profile - which includes informtion about each customer.
3. transcript - which includes data about the offer's and customer transactions.

These datasets are discussed in more details in my notebook.


### Problem Statement
Given the cutomers' information can we predict if a certain offer is successful or not. I believe that a successful offer must be received by the customer, then viewed, then completed within the offer's duration.

### Metrics
I will be utlizing the accuracy, recall, and f-score as evaluation metric for the different models that i will build.

### Data Sets

The data is contained in three files:

* portfolio.json - containing offer ids and meta data about each offer (duration, type, etc.)
* profile.json - demographic data for each customer
* transcript.json - records for transactions, offers received, offers viewed, and offers completed

## Data Exploration

#### Explaining the Portfolio Data

From the above we can see that the portfolio contains the following columns:
* id (string) - offer id
* offer_type (string) - type of offer ie BOGO, discount, informational
* difficulty (int) - minimum required spend to complete an offer
* reward (int) - reward given for completing an offer
* duration (int) - time for offer to be open, in days
* channels (list of strings)

It is good that there are no missing data in the portfolio

However, Since our objective (problem statement) is focused on BOGO, and discount offers. We will extract and drop all informational offer data.

#### Cleaning the Portfolio Data
1. Encoding offer_type using one-hat encode.
2. Cerate four new binary colum in replace to channels.
3. Normalize difficulty,duration,and reward columns.


#### Visualizing the Data

From the diffculty bar chart below we can see that the possible values are between 0 and 20. Therefore, I will normalize it.

{:refdef: style="text-align: center;"}
![Profile](/assets/s5.png)
{: refdef}



### Explaining the Profile Data

* age (int) - age of the customer
* became_member_on (int) - date when customer created an app account
* gender (str) - gender of the customer (note some entries contain 'O' for other rather than M or F)
* id (str) - customer id
* income (float) - customer's income




#### Cleaning profile data
1. Drop raws with missing valus.
2. Normilize income using min-max scale.
3. Normilize age using min-max scale.
4. use only the year on became_member_on
5. encode gender column


#### Visualizing the Data

From the income bar chart below we can see that the data is skewed and therefore I will normalize it.
{:refdef: style="text-align: center;"}
![Profile](/assets/s4.png)
{: refdef}


#### Explaining the Transcript Data

* event (str) - record description (ie transaction, offer received, offer viewed, etc.)
* person (str) - customer id
* time (int) - time in hours since start of test. The data begins at time t=0
* value - (dict of strings) - either an offer id or transaction amount depending on the record

#### Clean the transcirpt dataframe
the offer_id and the value are stored in the same column. Therefore, i will split them into two different columns.

#### determine if offer is successful
the offer must be recieved, viewed, then completed within the offer validity date to be considered successful

#### Creating a new dataframe that combines all the features prior to training
Now I will create a new dataframe that will combine all of the features so we can use it in training our model.

### Shuffle and Split Data
Now all categorical variables have been converted into numerical features, and all numerical features have been normalized. As always, we will now split the data (both features and their labels) into training and test sets. 80% of the data will be used for training and 20% for testing.

## Modeling and Evaluating Model Performance
In this section, we will investigate three different supervised learners and evaluate their performance in predict the success or failure of a project. Moreover, we will evaluate the model's performance using 1% of the data, 10% of the data, 100% of the data.


###  Supervised Learning Models
after trying a random subset of the data on the diffrante classifiers I have selected: Support Vector Machines, Random Forest, and Gaussian Naive Bayes.

Support Vector Machines
- SVM is applied on the field of image processing an example is face detection. [link](https://papers.nips.cc/paper/1609-support-vector-machines-applied-to-face-recognition.pdf)

- Weakness: high computational cost for training  

- strength: SVM will have one unique optimal solution since the problem will be convex. 

- Snice our problem is binary classification SVM might perform well


Random Forest
- A real-world application of Random Forest is  tumor classification based on tissue microarray data. [link](https://www.nature.com/articles/3800322)

- Weakness: The prediction time might be high compared to decision trees. 

- strength: Overcome the problem of overfitting on a normal decision tree.

- Snice our problem has a relatively good number of features I believe Random Forest will be good choose and perform better than normal decision tree.

Gaussian Naive Bayes
- A real-world application of Gaussian Naive Bayes classifier  is  in text categorization .[link]
(http://people.csail.mit.edu/jrennie/papers/icml03-nb.pdf)

- Weakness: assumes that features are independants. 

- strength:  It only requires a small amount of training data to estimate the parameters. [link]
(https://www.ic.unicamp.br/~rocha/teaching/2011s2/mc906/aulas/naive-bayes-classifier.pdf) .

- Since our data set is not that big in size Gaussian Naive Bayes might produce competitive results.




### Evaluation
{:refdef: style="text-align: center;"}
![Evaluattion](/assets/s1.png)
{: refdef}
From the visualizations above it looks like Random forst and SVC accurcy is almost the same when testing on 100% of the test data. the F score is shockingly very low for all the models. However, random forest is doing a little bit better than the others. that why i will try to imporve the results of Random Forest.


Decision trees learn by sorting the features based on the information gained(i.e. which will separate the data on better). After sorting the features, the algorithm will decide the threshold that separate the data best for each feature. In predicting the algorithm will check the feature on the predefined order(information gained) until it ends up on one of the class. Random forest creates multiple decision trees using random subsets of the data and random subset of the features for each tree. after creating all the decision trees the model will uses voting to predict the result. 


#### Fine-Tuning the Random Forest Classifier
By using grid search on two parameters, we have fine-tuned the random forest classifier. Below is the accuracy of the optimized model versus the unoptimized model versus naive predictor
{:refdef: style="text-align: center;"}
![Accuracy Comparison](/assets/s2.PNG)
{: refdef}


#### Feature Importance
The graph below shows the most important features in building our random forest model. As expected age and income are the top 2 featrues.
{:refdef: style="text-align: center;"}
![Accuracy Comparison](/assets/s3.png)
{: refdef}


## Conclusion

In this project I have built a model that predicts if an offer will be succesful given the targeted customer information and the offer details. This project followed four steps. In the begging, we have cleaned the data. After that we have constructed a new dataframe which includes all the features that will be used in training the different models. Next, we have trained three different models and evaluated thier performance. Finally, we improved the performance of the one of the selected models.

Although our model was able to predict the success of an offer with accuracy of more than 90%, I believe Starbuck should gather more information about thier customers like martial status, and accomadation location.