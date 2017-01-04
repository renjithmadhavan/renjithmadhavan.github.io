---
layout: post
title: Human Activity Recognition with Smartphones
excerpt: "Kaggle Project - ML Benchmark"
modified: 2016-12-28
categories: projects
tags: [python, kaggle]
image:
  feature: so-simple-sample-image-4.jpg
  credit: pixabay
  creditlink: https://pixabay.com/en/wave-atlantic-pacific-ocean-huge-1913559/
comments: true
share: true
---

# Human Activity Recognition with Smartphones

The Human Activity Recognition database was built from the recordings of 30 study participants performing activities of daily living (ADL) while carrying a waist-mounted smartphone with embedded inertial sensors. The objective is to classify activities into one of the six activities performed.

Reference: [Kaggle URL](https://www.kaggle.com/uciml/human-activity-recognition-with-smartphones)

## Description of experiment
The experiments have been carried out with a group of 30 volunteers within an age bracket of 19-48 years. Each person performed six activities (WALKING, WALKING_UPSTAIRS, WALKING_DOWNSTAIRS, SITTING, STANDING, LAYING) wearing a smartphone (Samsung Galaxy S II) on the waist. Using its embedded accelerometer and gyroscope, we captured 3-axial linear acceleration and 3-axial angular velocity at a constant rate of 50Hz. The experiments have been video-recorded to label the data manually. The obtained dataset has been randomly partitioned into two sets, where 70% of the volunteers was selected for generating the training data and 30% the test data.

The sensor signals (accelerometer and gyroscope) were pre-processed by applying noise filters and then sampled in fixed-width sliding windows of 2.56 sec and 50% overlap (128 readings/window). The sensor acceleration signal, which has gravitational and body motion components, was separated using a Butterworth low-pass filter into body acceleration and gravity. The gravitational force is assumed to have only low frequency components, therefore a filter with 0.3 Hz cutoff frequency was used. From each window, a vector of features was obtained by calculating variables from the time and frequency domain.

## Attribute information

### For each record in the dataset the following is provided:

1. Triaxial acceleration from the accelerometer (total acceleration) and the estimated body acceleration.
2. Triaxial Angular velocity from the gyroscope.
3. A 561-feature vector with time and frequency domain variables.
4. Its activity label.
5. An identifier of the subject who carried out the experiment.

## File Information
1. test.csv
2. train.csv

## CRISP Standard
The project will use the CRISP-DM industry standard workflow.

### Business Understanding
NA

### Data Understanding


```python
import numpy as np
import pandas as pd

train = pd.read_csv('../../../../input/human-activity/train.csv')
test  = pd.read_csv('../../../../input/human-activity/test.csv')
train.shape
```




    (7352, 563)



### Data Preparation

#### **Feature Engineering**

##### Tree_based feature selection

The ExtraTreesClassifier class from sklearn package can be used to do the tree based feature selection. As you can esee below the number of features is reduced to 92 from 562.


```python
import sklearn
from sklearn.ensemble import ExtraTreesClassifier
from sklearn.feature_selection import SelectFromModel
features = train.iloc[:,0:562]
label = train['Activity']
```


```python
clf = ExtraTreesClassifier(random_state=1)
clf = clf.fit(features, label)
model = SelectFromModel(clf, prefit=True)
new_features = model.transform(features)
print(new_features.shape)
```

    (7352, 92)


##### L1-Based feature selection
L1-Based feature selection resulted in the number of features reduced to 112 from 562.


```python
from sklearn.svm import LinearSVC
lsvc = LinearSVC(C=0.01, penalty="l1", dual=False, random_state=1).fit(features, label)
model_2 = SelectFromModel(lsvc, prefit=True)
new_features_2 = model_2.transform(features)
print(new_features_2.shape)
```

    (7352, 112)


### Modeling
The models we will use are:

1. DecisionTreeClassifier
2. RandomForestClassifier
3. GradientBoostingClassifier


```python
from sklearn.tree import DecisionTreeClassifier
from sklearn.ensemble import RandomForestClassifier,GradientBoostingClassifier
Classifiers = [DecisionTreeClassifier(),RandomForestClassifier(n_estimators=200),GradientBoostingClassifier(n_estimators=200)]
```

#### Base Lines Modeling - No feature Selection


```python
from sklearn.metrics import accuracy_score
import timeit
test_features= test.iloc[:,0:562]
Out_Accuracy_1=[]
for clf in Classifiers:
    tmp1=['no_sel', clf.__class__.__name__]
    start_time = timeit.default_timer()
    fit=clf.fit(features,label)
    pred=fit.predict(test_features)
    elapsed = timeit.default_timer() - start_time
    tmp1.append(elapsed)
    Model_1.append(clf.__class__.__name__)
    tmp1.append(accuracy_score(test['Activity'],pred))
    Out_Accuracy_1.append(tmp1)
```

#### Modeling using Tree Based Feature selection


```python
test_features= model.transform(test.iloc[:,0:562])
for clf in Classifiers:
    start_time = timeit.default_timer()
    tmp1=['tree', clf.__class__.__name__]
    fit=clf.fit(new_features,label)
    pred=fit.predict(test_features)
    elapsed = timeit.default_timer() - start_time
    tmp1.append(elapsed)
    tmp1.append(accuracy_score(test['Activity'],pred))
    Out_Accuracy_1.append(tmp1)
```

#### Modeling using L1-Based Featrue Selection


```python
test_features= model_2.transform(test.iloc[:,0:562])
for clf in Classifiers:
    start_time = timeit.default_timer()
    tmp1=['L1', clf.__class__.__name__]
    fit=clf.fit(new_features_2,label)
    pred=fit.predict(test_features)
    elapsed = timeit.default_timer() - start_time
    tmp1.append(elapsed)
    tmp1.append(accuracy_score(test['Activity'],pred))
    Out_Accuracy_1.append(tmp1)
```

### Evaluation

#### Accuracy


```python
import seaborn as sns
sns.set()
```


```python
df1 = pd.DataFrame(Out_Accuracy_1, columns=('Method', 'ML', 'elapsed', 'Score'))
df1
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Method</th>
      <th>ML</th>
      <th>elapsed</th>
      <th>Score</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>no_sel</td>
      <td>DecisionTreeClassifier</td>
      <td>5.491942</td>
      <td>0.855107</td>
    </tr>
    <tr>
      <th>1</th>
      <td>no_sel</td>
      <td>RandomForestClassifier</td>
      <td>25.615557</td>
      <td>0.927044</td>
    </tr>
    <tr>
      <th>2</th>
      <td>no_sel</td>
      <td>GradientBoostingClassifier</td>
      <td>375.824447</td>
      <td>0.937903</td>
    </tr>
    <tr>
      <th>3</th>
      <td>tree</td>
      <td>DecisionTreeClassifier</td>
      <td>0.849529</td>
      <td>0.832033</td>
    </tr>
    <tr>
      <th>4</th>
      <td>tree</td>
      <td>RandomForestClassifier</td>
      <td>9.610675</td>
      <td>0.900238</td>
    </tr>
    <tr>
      <th>5</th>
      <td>tree</td>
      <td>GradientBoostingClassifier</td>
      <td>72.704296</td>
      <td>0.923651</td>
    </tr>
    <tr>
      <th>6</th>
      <td>L1</td>
      <td>DecisionTreeClassifier</td>
      <td>1.053520</td>
      <td>0.843570</td>
    </tr>
    <tr>
      <th>7</th>
      <td>L1</td>
      <td>RandomForestClassifier</td>
      <td>10.631086</td>
      <td>0.908381</td>
    </tr>
    <tr>
      <th>8</th>
      <td>L1</td>
      <td>GradientBoostingClassifier</td>
      <td>79.728543</td>
      <td>0.923990</td>
    </tr>
  </tbody>
</table>
</div>




```python
g = sns.factorplot(x="Method", y="Score", col="ML",
                   data=df1, saturation=.5,
                   kind="bar", ci=None, aspect=0.6)
(g.set_axis_labels("", "Score")
 .set_xticklabels(["no_sel", "tree", "L1"])
 .set_titles("{col_name} {col_var}")
 .set(ylim=(0, 1))
 .despine(left=True))  
```




    <seaborn.axisgrid.FacetGrid at 0x7f62e44fbac8>




![png](/datascientist/figures/2016-12-28-human-activity-recognition-with-smartphones_20_1.png)


#### Running Time


```python
g = sns.factorplot(x="Method", y="elapsed", col="ML",
                   data=df1, saturation=.5,
                   kind="bar", ci=None, aspect=0.6)
(g.set_axis_labels("", "Elapsed")
 .set_xticklabels(["no_sel", "tree", "L1"])
 .set_titles("{col_name}")
 .despine(left=True))  
```




    <seaborn.axisgrid.FacetGrid at 0x7f62e4b2bf28>




![png](/datascientist/figures/2016-12-28-human-activity-recognition-with-smartphones_22_1.png)


### Summary

NA
