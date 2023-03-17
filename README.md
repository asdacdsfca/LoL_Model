# LoL Esports Matches Win Rate Analysis
by Andrew Zhao (yiz158@ucsd.edu)

Yiheng Yuan (yiy159@ucsd.edu)

___
## Introduction
- Our data contains the information of **LoL** (League of Legends, a popular moba games) **esports matches** in **2022**.
- The data is taken from [this website](https://oracleselixir.com/tools/downloads)
- Our **Prediction Question** for this project is:

### *Predict whether a player's role is support given their post-game data.* 

- **Type:** Classification
    - We chose classification because our prediction target is a categorical data type. Classification is a predictive model to identify discrete output variables, such as labels or categories.

    - **Binary Classification**
    Since we only predict whether the position is "support," each data sample will assign one and only one label from two mutually exclusive classes, such as true and false.

- **Response Variable:** position
    - We choose this variable because we are trying to predict if a player's position is "support" and this column exactly records each player's position information.

- **Metric Using:** Accuracy
    - We chose accuracy because we are interested in the proportion of predictions that are correct. It is reasonable to weight correct `position` predictions the same as all incorrect `position` predictions. In our case, incorrect position predictions are no worse than others, they do not tend to lead to a more serious consequence in any cases. 

- **Information at "Time of Prediction:**
    - Since our prediction of player's role is based on the post-game data, we would have everything except for the `position` column in the data set. To avoid overfitting, we decided to only use these features: [`patch`, `champion`, `position`, `kills`,`deaths`, `assists`, `dpm`, `damageshare`, `damagetakenperminute`, `vspm`, `earned gpm`, `cspm`].

___
## Baseline Model
- **Type of Features**
    - `patch`: Ordinal
    - `champion`: Nominal
    - `deaths`: Discrete
    - `kills`: Discrete
    - `assists`: Discrete
    - `dpm`: Continous
    - `damageshare`: Continous
    - `damagetakenperminute`: Continous
    - `vspm`: Continous
    - `earned gpm`: Continous
    - `cspm`: Continous
- **Number of Quantitive:** 9
- **Number of Ordinal:** 1
- **Number of nominal:** 1
- **Process of Encoding:**
    - Since we can't use categorical columns directly, we have to feature engineer `patch` and `champion`. Here we chose to use `OneHotEncode` ....
- **Current Condition of the Model:** While there are things that we can still improve, I think the baseline model is already a good representation of our final model's behavior. The reason is, we've included many representated features that could effectively demonstrate the behavior of `sup` roles. 

---
## Final Model
- **Features Added**
    - We chose to apply a `QuantileTransformer` on `vspm` and `earned gpm` because we noticed that if the `position` were `sup` they tend to have a much more higher `vspm` than other positions and significantly lower `earned gpm` than other positions, which means that the distribution of these two columns are not normal, leads to potential high bias in our predictions. 
    if we quantiles these two features then data in upper quantile of `vspm` and in lower quantile of `earned gpm` are likely to be `sup`. 

    - We chose to apply a `FunctionTransformer` on `dpm`, which computes $log(dmp+1)$ because we noticed that `sup` tends to deal much low `dpm`. However, based on our observation, if a player deals a very high dmp, for example, 500 should be weight similarily to dpm 1200 and 

    - We chose to apply a `FunctionTransformer` on `kills` and `assists`, which computes $(kills+1)/(assists+1)$. This helps our prediction because we observed that `sup` tends to have more `assits` than the `kills` they have. 

- **Search for Best Model**
    - We chose to use  


