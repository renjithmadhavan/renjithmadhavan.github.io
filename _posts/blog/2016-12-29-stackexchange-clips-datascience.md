---
layout: post
title: Stack Exchange Clips for Data Science
excerpt: "Stack Exchange Clips"
modified: 2016-12-29
categories: blog
tags: [Data Science, Stackexchange]
image:
  feature: so-simple-sample-image-4.jpg
  credit: pixabay
  creditlink: https://pixabay.com/en/wave-atlantic-pacific-ocean-huge-1913559/
comments: true
share: true
---

Below is a collection of stackexchange clips that I found useful while working with Data.
Each clip is hyperlinked to the original stackexchange post for reference.

#### [Stack1](http://stackoverflow.com/questions/38105539/how-to-convert-a-scikit-learn-dataset-to-a-pandas-dataset)

How to convert a Scikit-learn dataset to a Pandas dataset?

```python
import numpy as np
import pandas as pd
from sklearn.datasets import load_iris

# save load_iris() sklearn dataset to iris
# if you'd like to check dataset type use: type(load_iris())
# if you'd like to view list of attributes use: dir(load_iris())
iris = load_iris()

# np.c_ is the numpy concatenate function
# which is used to concat iris['data'] and iris['target'] arrays 
# for pandas column argument: concat iris['feature_names'] list
# and string list (in this case one string); you can make this anything you'd like..  
# the original dataset would probably call this ['Species']
data1 = pd.DataFrame(data= np.c_[iris['data'], iris['target']],
                     columns= iris['feature_names'] + ['target'])
```
