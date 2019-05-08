# MLFeatureSelection
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![PyPI version](https://badge.fury.io/py/MLFeatureSelection.svg)](https://pypi.org/project/MLFeatureSelection/)

General features selection based on certain machine learning algorithm and evaluation methods

**Divesity, Flexible and Easy to use**

More features selection method will be included in the future!

## Quick Installation

```python
pip3 install MLFeatureSelection
```

## Modulus in version 0.0.9.5.1

- Modulus for selecting features based on greedy algorithm (from MLFeatureSelection import sequence_selection)

- Modulus for removing features based on features importance (from MLFeatureSelection import importance_selection)

- Modulus for removing features based on correlation coefficient (from MLFeatureSelection import coherence_selection)

- Modulus for reading the features combination from log file (from MLFeatureSelection.tools import readlog)

## This features selection method achieved

- **1st** in Rong360

-- https://github.com/duxuhao/rong360-season2

- **6th** in JData-2018

-- https://github.com/duxuhao/JData-2018

- **12nd** in IJCAI-2018 1st round

-- https://github.com/duxuhao/IJCAI-2018-2

## Modulus Usage

[Example](https://github.com/duxuhao/Feature-Selection/blob/master/Demo.ipynb)

- sequence_selection

```python
from MLFeatureSelection import sequence_selection
from sklearn.linear_model import LogisticRegression

sf = sequence_selection.Select(Sequence = True, Random = True, Cross = False) 
sf.ImportDF(df,label = 'Label') #import dataframe and label
sf.ImportLossFunction(lossfunction, direction = 'ascend') #import loss function handle and optimize direction, 'ascend' for AUC, ACC, 'descend' for logloss etc.
sf.InitialNonTrainableFeatures(notusable) #those features that is not trainable in the dataframe, user_id, string, etc
sf.InitialFeatures(initialfeatures) #initial initialfeatures as list
sf.GenerateCol() #generate features for selection
sf.SetFeatureEachRound(50, False) # set number of feature each round, and set how the features are selected from all features (True: sample selection, False: select chunk by chunk)
sf.clf = LogisticRegression() #set the selected algorithm, can be any algorithm
sf.SetLogFile('record.log') #log file
sf.run(validate) #run with validation function, validate is the function handle of the validation function, return best features combination
```

- importance_selection

```python
from MLFeatureSelection import importance_selection
import xgboost as xgb

sf = importance_selection.Select() 
sf.ImportDF(df,label = 'Label') #import dataframe and label
sf.ImportLossFunction(lossfunction, direction = 'ascend') #import loss function and optimize direction
sf.InitialFeatures() #initial features, input
sf.SelectRemoveMode(batch = 2)
sf.clf = xgb.XGBClassifier() 
sf.SetLogFile('record.log') #log file
sf.run(validate) #run with validation function, return best features combination
```

- coherence_selection

```python
from MLFeatureSelection import coherence_selection
import xgboost as xgb

sf = coherence_selection.Select() 
sf.ImportDF(df,label = 'Label') #import dataframe and label
sf.ImportLossFunction(lossfunction, direction = 'ascend') #import loss function and optimize direction
sf.InitialFeatures() #initial features, input
sf.SelectRemoveMode(batch = 2)
sf.clf = xgb.XGBClassifier() 
sf.SetLogFile('record.log') #log file
sf.run(validate) #run with validation function, return best features combination
```

- tools.readlog: read previous selected features from log

```python
from MLFeatureSelection.tools import readlog

logfile = 'record.log'
logscore = 0.5 #any score in the logfile
features_combination = readlog(logfile, logscore)
```

- tools.filldf: complete dataset when there is cross-term features

```python
from MLFeatureSelection.tools import readlog, filldf

def add(x,y):
    return x + y

def substract(x,y):
    return x - y

def times(x,y):
    return x * y

def divide(x,y):
    return x/y

def sq(x,y):
    return x ** 2


CrossMethod = {'+':add,
               '-':substract,
               '*':times,
               '/':divide,
               } # set your own cross method

df = pd.read_csv('XXX')
logfile = 'record.log'
logscore = 0.5 #any score in the logfile
features_combination = readlog(logfile, logscore)
df = filldf(df, features_combination, CrossMethod)
```

- format of validate and lossfunction

define your own:

**validate**: validation method in function , ie k-fold, last time section valdate, random sampling validation, etc

**lossfunction**: model performance evaluation method, ie logloss, auc, accuracy, etc

```python
def validate(X, y, features, clf, lossfunction):
    """define your own validation function with 5 parameters
    input as X, y, features, clf, lossfunction
    clf is set by SetClassifier()
    lossfunction is import earlier
    features will be generate automatically
    function return score and trained classfier
    """
    clf.fit(X[features],y)
    y_pred = clf.predict(X[features])
    score = lossfuntion(y_pred,y)
    return score, clf
    
def lossfunction(y_pred, y_test):
    """define your own loss function with y_pred and y_test
    return score
    """
    return np.mean(y_pred == y_test)
```

## multiple processing 

Multiple processing can be set in validate function when you are doing N-fold.
    
## DEMO

More examples are added in example folder include:

- Demo contain all modulus can be found here ([demo](https://github.com/duxuhao/Feature-Selection/blob/master/Demo.ipynb))

- Simple Titanic with 5-fold validation and evaluated by accuracy ([demo](https://github.com/duxuhao/Feature-Selection/tree/master/example/titanic))

- Demo for S1, S2 score improvement in JData 2018 predict purchase time competition ([demo](https://github.com/duxuhao/Feature-Selection/tree/master/example/JData2018))

- Demo for IJCAI 2018 CTR prediction ([demo](https://github.com/duxuhao/Feature-Selection/tree/master/example/IJCAI-2018))


## Function Parameters

[Parameters](https://github.com/duxuhao/Feature-Selection/blob/master/MLFeatureSelection)

## Algorithm details

[Details](https://github.com/duxuhao/Feature-Selection/blob/master/Algorithms_Graphs)
