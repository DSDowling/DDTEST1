# Inspecting a model

The Model Summary page, which can be seen by clicking on a model card or list entry in the Models page, provides three insights on the model:

* The **score** of the model - according to the score metric that was used to train it. Read more about [classification](../theory/classification-metrics.md) and [regression](../theory/classification-metrics.md) metrics.
* A report on the **health** of the model - which tells you whether issues where found during training that make the model sub-optimal. If this is the case, it is still possible to use the model, but it is likely that changing something in the training process may lead to better results. The {{ book.platform }} adviser will identify these issues and suggest possible solutions.
* The **influences** of the model - a detailed view of which columns had an impact on the model performance.

![Insight Summary](../assets/trained-models/insight-summary.png)

## Model Health

Model health tests whether your model has extracted any statistically significant predictive performance out of the columns available to it in the training data:
 * If your model health is good (green tick) you can conclude that your model has found some predictive value in your data.
 * If your model health is bad (red cross) you can conclude that the model has not found any patterns in the data to help predict your target.

Read more about [how model health is computed](../theory/model-health.md).

### What to do if model health is bad

Bad model health is an indication that your data (your columns and rows) is not rich enough to create a predictive model for your target column. Here are two examples of ways in which you could improve this situation:
 * **Get more columns:** it may be that your columns do not contain any predictive information about your target. In this case, think again about what other data you have which could be predictive of your target. If you can isolate some more data, you should add and re-run model building.
 * **Get more rows:** it may be that your columns contain predictive information about your target, but this predictive signal is difficult to uncover because the model does not have enough examples (rows) to find patterns. In this case, you should think about whether you can find more examples to train the model with. If you can find more examples, you should add them to your data and re-run model building.

> **Note**  
> It is **not advisable** to re-run the model training without changing one of these things (read more about it [here](../theory/model-health.md#why-you-shouldnt-just-re-run-without-changing-anything)). Equally, you should not ignore bad model health unless some specific circumstances are met (read more about these [here](../theory/model-health.md#when-is-it-safe-to-ignore-bad-model-health).

## Model Influences

Model influences tell us **how important** each column in a data set is for predicting the target column. This can be immensely useful when deciding what data should be collected and what can be left out.

Clicking on the Model Influences card takes us to a more detailed view:

* In a classification model you have the ability to switch between the target classes. Use the **Predicted Category** dropdown at the top of the page to do this. In a regression problem, this is not present as you are predicting a continuous variable (for example, the value of a house or stock prices).
* The **Influence** bar tell us how important that column is for predicting your target. Columns are sorted by importance, so the most important will come out on top.
* The **plot** tells you how confident the model is that a **particular value** of that column has a high influence on the prediction. 
See the worked example below. The plot comes in two forms, a **line plot** for continuous features, and a **box plot** for discrete features.

![Influences](../assets/trained-models/insight-influences.png)

### Worked Example

In the screen capture above, we are predicting whether a set of clients are going to leave a mobile operator or remain with them. A prediction of this 'churn' is marked as "TRUE" and not churn as "FALSE".

In this example, the *customer service calls* is the most important column, with an influence of 15% (of the total influence given by all columns in the dataset). Of the *customer service calls* values present in the training data set, we can see that low values (closer to 0) swing the model towards a prediction of false/not churning. This is represented as a 'box plot' because the *customer service calls* is a categorical variable (it only has a certain number of values). The top and bottom margins of the box represent the variance in the values and in the continuous plot as the shaded surface around the median (the solid line), the edges of which also represent first and third quartiles. If this is large at a given value, it means that that value has less impact on the prediction.

The *international plan* feature in this example only has two values, and is therefore also *categorical*. We can see that having an international plan makes the customer more likely to churn, and vice versa.

Finally, the *total day charge*, third most important feature, has an influence of 9.9%, and similarly *total day minutes* of 9.8%. Again, on the range of values of *total day charge* present in the training data set, i.e. between 0 and 60, we can see that high values (towards 60) swing the model more towards a prediction of true, i.e. of churning, than low values.

## Model Influences on Tests

Another way to see the model influences is to inspect the output of a [Test](test.md), including a **Hold Out** test, as depicted below.

![Insights on Hold-out test](../assets/trained-models/insight-on-holdout.png)
