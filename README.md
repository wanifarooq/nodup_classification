


## Project Overview

The problem statement is to solve the classification task for the given dataset. The dataset is the JSON
file containing many entries where each entry belongs to one or another class. We have to devise the
algorithmic procedure to classify a similar type of entry where the class of entry is missing.

## Dataset

The training dataset contains 6073 distinctive records. The Dataset is of the form of the JSON structure
where the general structure is:

{ "id": "containing the id of record",
"semantic": “containing the class label”
“lista_asm” : “containing the list of instructions”
“cfg” : “containing the control flow graph information”

Firsly , I loaded the dataset in my python project and renamed some columns which are given as:

columns = {'lista_asm': 'Instructions', 'cfg': 'FlowGraphs' ,'semantic' : 'Class'}

I have captured the general highlight of the data using the pandas-profiling feature of the python and
the following results I got from it.

Here you can see that we have information on only the three variables though we have four variables in
the original JSON from the dataset. This is because I had eliminated the column named “Id” as it is not
going to contribute to the classification.
Other interesting stats I got from the Data are :

Here we got the confirmation that every record has unique values of instructions and the Flowgraphs;
The (“cfg” ) column is not understandable to python right now as it is a packed object and needs to be


unpacked.

The number of entries or rows for each class by numbers are as:

The percentage contribution of each of the Classes is given in the following pie chart, it is evident that
the contribution by the “sort “ class is very less compared to the other, which means we have a slightly
unbalanced dataset.

we also got the varying lengths of the instructions for every entry which are given as :

Hence from this observation, it is evident that we can use this property as one of the features for


classification.

The Matrix plot of the dataset shows that we don't have any missing values.

Hence our dataset is complete so we don't have to tackle the missing value problem.

## Preprocessing and Feature Extraction

### After analyzing the data I used two separate procedures to get the features for the classification

### algorithm :

### Firstly, I used the FlowGraph information to get the three features and those three features are :

### Node_length: gives the number of nodes that are present in the instruction set.

### Double_length: gives the Ratio of the total nodes (node_length) to the number of bi-directional

### nodes.

### Single_length: gives the Ration of the total nodes to the number of unidirectional Nodes.

### Secondly, I used the instruction information, there I used the Vectorization technique of Td_idf.

### The Td_idf is the term document _inverse document frequency.I looped over each row got the

### instructions set and parsed it to get its operators then merged all these operators present in one

### row or entry, After that, I sent this Information to the Vectorization function to make the vectors

### of it. The function returns the frequency of each instruction of assembly language in each row,

### as I also used the ngram(1,3) it also kept the order and sequence of terms for one to three units

### which resulted in a much bigger vector than the number of unique terms.

### After that, I used the Hstack to combine these two feature techniques as the output from these

### techniques were of different formats.

### Now I have the Feature dataset ready I need to divide it into the training and the testing sets.

### Here used the help of Sklearn and used its function to give me the training dataset and testing

### dataset.


## MODEL

I selected the logistic regression model of the sklearn library and passed the training dataset without
any parameter tuning and the results were quite bad as expected. The Ngram used for this first training
was (1,1)

precision recall f1-score support
encryption 0.89 0.87 0.88 312
math 1.00 0.96 0.98 719
sort 0.00 0.00 0.00 167
string 0.75 0.99 0.85 624

accuracy 0.87 1822
macro avg 0.66 0.70 0.68 1822
weighted avg 0.80 0.87 0.83 1822

after getting this bad accuracy and f1-score, I find out that the main cause of this lower score is that the
“sort' and “string” classes are using an almost similar type of assembly instruction.

Therefore I changed the ngram parameters, where now not only the number of instructions contribute
but also the sequence of instructions will also contribute to classification.

After doing that, the results saw an improvement as shown below:

but still, the performance was low so I tried a different solver and replaced the saga solver with the lib
linear solver.
There was an improvement but still, it was not that high, I saw that by default this solver uses the L
regularization.
As I was not overfitting I replaced that with L1 regularization and the results are better.


## Evaluation and Visualization

I used the confusion matrix and other evaluation techniques to check the results and the weights of the
classifier. I also tried to see whether the features I extracted from flow graphs are contributing to the
results or not. I used the three very important libraries from python which are Sklearn for confusion
matrix, yellow-brick for different visualizations, and the most important eli5 for checking the feature
importance.

The results are discussed below:
This is the confusion matrix as we can see that most of the entries are classified correctly, though there
is scope for improvement.

The yellow-brick visualization of the test set shows that the “sort” has minimum accuracy out of all:


To get a more intuitive understanding the result can be viewed as:

The bar chart view of the above solution can be viewed as :

The Area under the curve of evaluation can be viewed as :

The evident thing from that is the result given by the model is very good and the "sort" class has a
the minimum area under its curve which is expected as it has the lowest accuracy score out of all the four
classes.


## Feature importance

Finally, to check which features are contributing to the result I have used the eli5 to check the weights
of each feature and the result is as shown in fig:


This proves that our method of extracting features like node_length, double_length, and single_length
is contributing to the final result of classification and have very good positive weights for math and
encryption classes. At the same time to classify sort and string many more features are considered and
have further scope for improvement.
Finally, I want to show how these weights contribute by applying them to one example.


