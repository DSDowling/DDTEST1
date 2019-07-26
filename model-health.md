# Model Health

Model health tests whether your model has extracted any statistically significant predictive performance out of the columns available to it in the training data:
 * If your model health is good (green tick) you can conclude that your model has found some predictive value in your data.
 * If your model health is bad (red cross) you can conclude that the model has not found any patterns in the data to help predict your target.

### How Model Health is Computed

Model health is a comparison between a naïve model, which is built using only the target column as training data, and the model created by model building, which is built using all the columns within the training data. A statistical test is conducted on the hold-out data to decide if the model built during model Building has better performance than the naïve model.

**Creating a naïve model:**
 * **Naïve classification model:** We build a posterior distribution using a Dirichlet prior distribution and the class frequencies in the original target column of the training data. The posterior forms the naïve model, and predictions for any new feature vector are made by sampling a class from the posterior.
 * **Naïve regression model:** The training data is used as an empirical distribution to approximate future values of the data. Predictions are made by re-sampling at random from the training data.

Note that, in both cases, the prediction is independent of the values of the feature vector.

**Computing model health:** Having built a naïve model, predictions are made on the hold-out data by both the naïve model, and the model created by model building. If the error of the the naïve model is better than the model created by model building more than 1% of the time, the created model is marked as having bad model health.

### What to do if model health is bad
Bad model health is an indication that your data (your columns and rows) is not rich enough to create a predictive model for your target column. Here are two examples of ways in which you could improve this situation:
 * **Get more columns:** it may be that your columns do not contain any predictive information about your target. In this case, think again about what other data you have which could be predictive of your target. If you can isolate some more data, you should add and re-run model building.
 * **Get more rows:** it may be that your columns contain predictive information about your target, but this predictive signal is difficult to uncover because the model does not have enough examples (rows) to find patterns. In this case, you should think about whether you can find more examples to train the model with. If you can find more examples, you should add them to your data and re-run model building.

> **Note**  
> It is **not advisable** to re-run the model training without changing one of these things. Equally, you should not ignore bad model health unless some specific circumstances are met. You can read more about this below. 

#### Why you shouldn't just re-run without changing anything
As the test for model health is based on a frequentist statistic, it is susceptible to p-hacking. Essentially, p-hacking means that if you repeat the process sufficiently many times, model health will eventually be flagged as good by chance. This is unlikely to happen unless you perform a lot of re-runs, but every time you do a re-run still diminishes the value of the test.

#### When is it safe to ignore bad model health
As a general rule, it is not safe to ignore bad model health. However, if your data is small, and you know that the features contain only a little predictive information about the target, it may be safe to ignore bad model health. This is because the test is sensitive to the number of examples in that make it into your hold-out data.
 * **In regression** this is unlikely to happen unless you have a very small amount of rows in the data available to model-building (about 100 rows or less).
  * **In classification** the problem is more subtle, as it is the size of the smallest class to which the test is sensitive. Specifically, if you have about 100 or fewer examples of the smallest class, then this problem is more likely to occur. Thus, if you are, for example, predicting fraud, and you have a data set of 1,000,000 transactions, only 100 of which are fraudulent, the model health test is likely to be compromised.

If you cannot find more data to enlarge your data set, then it may be accpetable to ignore bad model health until such data is available.
