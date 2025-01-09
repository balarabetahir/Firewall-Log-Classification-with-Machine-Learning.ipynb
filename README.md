Firewall Log Classification with Machine Learning
Machine learning is powerful, but it's not magic. It doesn't just solve problems for you. You have to approach it systematically, especially when dealing with complex domains like network security. This Project- focused on classifying network firewall data - is a good example of what it takes to build something that works.
The Problem
Firewalls are critical to network security. They decide what traffic to allow or block, based on a set of rules. But manually analyzing firewall data to detect patterns or anomalies is time-consuming and error-prone. So the goal here was to automate that process using machine learning. Specifically, we wanted to classify traffic into categories like "allow," "deny," "drop," or "reset-both" based on a dataset of firewall logs.
The Dataset
The dataset came from the UCI Machine Learning Repository. It's a subset of firewall logs with 12 features. Some are obvious, like source and destination ports or bytes sent and received. Others, like NAT ports, are less intuitive but useful for tracking traffic patterns. The target variable, "action," tells us what the firewall decided to do with each entry.
Input features (column names):

Source Port - a port, from which data transfer has been carried out

Destination Port - a port, to which data transfer has been carried out

NAT Source Port - a port, which needs to track for outgoing traffic from NAT (Network Address Translation) side to retransfer transit packets

NAT Destination Port - a port, which needs to track for incoming traffic from NAT (Network Address Translation) side to retransfer transit packets

Action - a feature (desired target), which is used as a class, 4 classes in total (allow, deny, drop, and reset-both)

Bytes - bytes total, is equal to the sum of the next two columns

Bytes Sent - the amount of data sent in bytes

Bytes Received - the amount of data received in bytes

Packets – packets total, is equal to the sum of the last two columns

Elapsed Time (sec) - time taken to transfer data in seconds

pkts_sent - the amount of data sent in packets

pkts_received - the amount of data received in packets

To see general information on the all dataframe features (columns), we use the info method:
Like many real-world datasets, it wasn't perfect. The classes were imbalanced, with most entries labeled as "allow." A tiny fraction were labeled "reset-both," making them almost invisible in the original data. This imbalance makes naive models ineffective because they'll just predict the majority class every time. Fixing this required careful preprocessing.
The Approach
The workflow was standard but thorough:
Exploration: First, understand the data. Look at shapes, column types, and class distributions. Know what you're dealing with before touching it.
Preprocessing:

Normalize column names to avoid confusion later.
Convert the categorical target variable into numbers.
Remove outliers using Tukey's method. This step cut the dataset nearly in half but left it cleaner.
Use SMOTE to balance the classes by oversampling minority ones.
Split the data into training and test sets, then scale the features.

3. Model Training: Eleven different classifiers were available. You could try them all, but it made more sense to focus on a few: Random Forest, k-Nearest Neighbors (kNN), and Extra Trees performed well historically.
4. Evaluation:
Calculate accuracy for both training and test sets.
Measure log loss to see how well models handle uncertainty.
Log correct and incorrect predictions for context.

=============================================
****    Estimations    ****
Test accuracy:  90.09%    Name: Linear_Regressor
Train accuracy:  89.61%
Log_loss:  0.2822
Correct   points:  17999
Incorrect points:  1980
=============================================
****    Estimations    ****
Test accuracy:  74.67%    Name: Random_Forest
Train accuracy:  99.98%
Log_loss:  0.3296
Correct   points:  14919
Incorrect points:  5060
=============================================
****    Estimations    ****
Test accuracy:  81.08%    Name: Ada_Boost
Train accuracy:  99.98%
Log_loss:  3.9102
Correct   points:  16198
Incorrect points:  3781
=============================================
****    Estimations    ****
Test accuracy:  99.16%    Name: Ex_Tree_Classifier
Train accuracy:  99.98%
Log_loss:  0.0918
Correct   points:  19811
Incorrect points:  168
=============================================
****    Estimations    ****
Test accuracy:  99.45%    Name: kNN
Train accuracy:  99.56%
Log_loss:  0.0968
Correct   points:  19869
Incorrect points:  110
=============================================
****    Estimations    ****
Test accuracy:  73.72%    Name: Decision_Tree
Train accuracy:  99.98%
Log_loss:  9.4714
Correct   points:  14729
Incorrect points:  5250

Elapsed time: 20.97 sec
What Worked
Some classifiers shone, while others stumbled:
Extra Trees Classifier and kNN consistently performed well on test accuracy. They generalized better than most.
Models like Random Forest and AdaBoost nailed training accuracy but faltered on test data. This is classic overfitting: memorizing the training set instead of learning patterns.
Log loss highlighted subtle differences in performance. Models with low log loss (like Extra Trees) weren't just accurate; they were confident in their predictions.

What Didn't Work
Overfitting was a recurring issue. Random Forest scored near-perfect on training data but stumbled on unseen examples. This reminds us that a high training accuracy is meaningless if the model doesn't generalize.
The class imbalance also created problems early on. Without SMOTE, most models ignored minority classes entirely. Balancing the data was essential but not a silver bullet - you still had to tune each model carefully.
Lessons Learned
Data Rules Everything: Garbage in, garbage out. Preprocessing isn't glamorous, but it's where most of the work happens. Fixing outliers and balancing classes mattered more than any specific algorithm.
No One-Size-Fits-All Model: Different problems need different tools. Even in this lab, the "best" classifier depended on your metric. Extra Trees had great accuracy, but kNN was almost as good and simpler to understand.
Metrics Matter: Accuracy alone doesn't tell the whole story. Log loss revealed differences that accuracy missed. Overfitting showed up clearly when comparing training vs. test results.
Visualize Everything: Seeing the results made patterns obvious. Bar charts comparing accuracy or log loss helped pinpoint the best models quickly. If the logging DataFrame isn't set up right, fix it. Without visualization, it's easy to miss what's happening.

Moving Forward
This project is a microcosm of machine learning in practice. It's not about chasing the latest algorithm or dataset. It's about methodically solving problems:
Understand your data.
Preprocess it thoughtfully.
Experiment with models, but don't blindly trust them.
Measure everything and focus on results that matter.

The real world is messy. This Project- with its imbalanced classes, noisy data, and overfitting models - is a good reminder that machine learning is messy too. But it's also rewarding if you're willing to put in the work.
Frequently Asked Questions: Firewall Log Classification
What is the primary goal of this Project?
 The main objective of this lab is to develop the most effective classifier for network security, specifically using the Internet Firewall Dataset. This involves exploring the dataset, selecting an appropriate classifier from a set of pre-existing options, calculating and analyzing performance metrics, and visualizing the results to make informed decisions.
What kind of data is used in this analysis? 
The dataset used is a subset of the Internet Firewall Dataset from the UCI Machine Learning Repository. This dataset contains information about network traffic, including source and destination ports, NAT configurations, actions taken (allow, deny, drop, reset), data volumes, time elapsed, and packet counts. The data is a log of firewall activities.
What is a classification task in machine learning, and how does it apply here?
 In machine learning, a classification task involves assigning input data to one or more predefined categories or classes. In this lab, the classification task is to predict the 'Action' (allow, deny, drop, or reset-both) taken by the firewall based on the various network traffic features provided. The goal is to build an algorithm that can correctly categorize firewall logs based on input data.
What is supervised learning, and why is it relevant to this lab?
 Supervised learning is a type of machine learning where the algorithm learns from labeled data, meaning the desired output (target variable) is known. Classification is a sub-category of supervised learning. Here, the "Action" feature is the labeled data, which the classifiers use to learn the relationships between network traffic features and firewall actions. This enables the model to predict the class of future unlabeled data.
How is data preprocessed before training the classifiers?
 The data preprocessing steps include:
Encoding the categorical 'action' feature into numerical values using LabelEncoder.
Identifying and removing outliers using Tukey's method based on quantiles.
Addressing imbalanced data issues through oversampling using the SMOTE technique.
Splitting the data into training and testing sets, while ensuring stratification (maintaining the proportion of classes) across sets.
Applying standardization (scaling) to both training and testing sets so the features have zero mean and unit variance. This improves the performance of many machine learning algorithms.

What are the metrics used to evaluate the performance of classifiers? Several key metrics are used to assess classifier performance:
Test Accuracy: The proportion of correct predictions made by the classifier on the test dataset.
Train Accuracy: The proportion of correct predictions made by the classifier on the training dataset.
Log Loss: A metric that measures the uncertainty of the probabilistic predictions made by a model. Lower log loss values indicate better model performance.
Correct Cases: The number of instances correctly predicted by the classifier.
Incorrect Cases: The number of instances incorrectly predicted by the classifier. These metrics help in understanding how well the classifier is generalizing to unseen data and also in identifying overfitting issues.

How is the best classifier chosen? 
The best classifier is selected based on the analysis of performance metrics, primarily by looking for the highest test accuracy, lowest log loss, and visualizing results to identify the most performant and stable classifier. The overall goal is to achieve high accuracy, minimize uncertainty, and generalize well on the test data.
What types of visualizations are used for comparison?
 Bar plots are used to visualize and compare the performance of different classifiers. These plots display test accuracy, train accuracy, log loss, number of correct cases, and number of incorrect cases, facilitating the selection of the best performing classifier. These plots helps to identify at a glance which classifier has the best performance and which aspects to focus on to improve classification results.
