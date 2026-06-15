# Machine Learning Comparative Analysis Exercise
In this file, im doing an exercise to compare analysis of data using Rapidminer and Phyton

There are 3 cases using Titanic dataset, Iris dataset, and Wisconsin Breast Cancer Dataset.

## Titanic Dataset
- Rapidminer
  In the tab process, the design begins with retrive titanic dataset, select attributes (survived, age, sex). For the preprocessing, it is mandatory to replace missing values and normalize the data. Then, the data is splitted and inputted to Logistic Regression, then connect both split data and logistic regression to apply model. the performance is then can be examined in the results tab, in the performance vector.
- Python
  Vibe coding was used in this process, but all things are checked so everything is within control, and logistic regression is also used when using python

## Iris Dataset
- Rapidminer
  The design is much simpler because it only uses 3 process of retrieve dataset, set roles (id, label), and cross validation to get the results. Inside the cross validation, i used random forest algorithm for the training, then in the testing i put the apply model and performance.
- Python
  For the python i used RandomForestClassifier, but also added SVC model for comparison. the result is slightly higher.

## Wisconsin Breast Cancer Dataset
- Rapidminer
  The design in this process was a bit complex than the other. Retrieve dataset, preprocess (select attributes, set role, normalize), then the data is splitted into training and testing using two blocks of cross validation because it was required to use two models at the same time (Decision Tree and SVM)
- Python
  Using python i only used SVM model and achieved similar results compared to SVM in rapidminer
