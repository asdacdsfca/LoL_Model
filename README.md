# LoL Esports Model Building For Predict Support Position
by Andrew Zhao (yiz158@ucsd.edu)

Yiheng Yuan (yiy159@ucsd.edu)

Our exploratory data analysis on this dataset can be found [here](https://asdacdsfca.github.io/LOL_Esports_Analysis/).
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
    - We chose to apply a `QuantileTransformer` on `vspm` and `earned gpm` because we noticed that those two columns contain some outliers that is not representative and we want to improve our model to be more accurate and more representative, so we use `QuantileTransformer` to remove those outliers.

    - We chose to apply a `FunctionTransformer` on `kills` and `assists`, which computes $(kills+1)/(assists+1)$. This helps our prediction because we observed that `sup` tends to have more `assits` than the `kills` they have. 

- **Search for Best Model**
    - We chose to use Decision Tree Classification as our model. The decision-tree algorithm is a constructive one that aids essential decision-making processes. Since we aim to predict a true-false question, the decision tree best fits and answers it.
    - The hyperparameters that ended up performing the best is: `{'tree__criterion': 'gini', 'tree__max_depth': 5, 'tree__min_samples_split': 10}`
    - The method we used to select hyperparameters is `GridSearchCV`
    - The overall model is that we transform all catergorical columns, `patch` and `champion`, into numerical column by apply `OneHotEncoding` Transformer, and then used `QuantileTransformer` to make the `vspm`, stands for "version score per minute" , more into a normal distribution, and used log inside a `FunctionTransformer` to make `dpm`, stands for "damage per minute", less skewed, and used another `FunctionTransformer` to find the `kills` per `assists` from the given dataset. Finally, we use `DecisionTreeClassifier` with the best hyperparameters we found from `GrodSearchCV` to created our final model.
    - When we comparing the score from both the baseline model and final model, we see the result has a slight improvement. This is because we made our numerical data more representable then before after those new transformations.

## Fairness Analysis

- **Group X:** player who pick champion that is defualt as a support champion by league of legends offically

- **Group Y:** player who pick champion that is not defualt as a support champion by league of legends offically

Reference URL: https://www.leagueoflegends.com/en-us/champions/

- **Evaluation Metric:** Accuracy

- **Null Hypothesis:** Our model is fair. Its accuracy for player who choose a support champion and player who choose a non-support champion are roughly the same, and any differences are due to random chance.

- **Alternative Hypothesis:** Our model is unfair. Its accuracy for player who choose a support champion is lower than its precision for player who choose a non-support champion.

- **Test statistic:** Difference in accuracy (support minus non-support).

- **Significance level:** 0.05.

- **Conculsion:** After the permutation test and observed the p-value, it seems like the difference in accuracy across the two groups is significant.