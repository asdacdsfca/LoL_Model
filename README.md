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
    We choose this variable because we are trying to predict if a player's position is "support" and this column exactly records each player's position information.

- **Information at "Time of Prediction:**
    Since our prediction of player's role is based on the post-game data, we would have everything except for the `position` column in the data set. To avoid overfitting, we decided to only use these features: [`patch`, `champion`, `position`, `kills`,`deaths`, `assists`, `dpm`, `damageshare`, `damagetakenperminute`, `vspm`, `earned gpm`, `cspm`].


