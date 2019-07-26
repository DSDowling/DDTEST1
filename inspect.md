# Inspecting a model

The Model Summary page, which can be seen by clicking on a model card or list entry in the Models page, provides three insights on the model:

* The **score** of the model, according to the score metric that was used to train it. Read more about [classification](../theory/classification-metrics.md) and [regression](../theory/classification-metrics.md) metrics.
* A report on the **health** of the model, which tells us whether issues where found during training that make the model sub-optimal. If this is the case, it is still possible to use the model, but it is likely that changing something in the training process may lead to better results. The {{ book.platform }} adviser will identify these issues and suggest possible solutions.
* The **influences** of the model, a detailed view on what features had impact on the model performance.

![Insight Summary](../assets/trained-models/insight-summary.png)

## Model Health

Model health tests whether your built model has extracted any statistically significant predictive performance out of the features available to it in the training data:
 * If your model health is good (green tick) you can conclude that your model has found some predictive value from in features for predicting your target.
 * If your model health is bad (red cross) you can conclude that the model has not found any patterns in the features which are predictive of your target.

Read more about [how model health is computed](../theory/model-health.md).

### What to do if model health is bad

Bad model health is an indication that your feature data is not rich enough to create a predictive model for your target column. There are two, non-exclusive ways in which you can improve this situation:
 * **Get more features:** it may be that your features do not contain any predictive information about your target. In this case, you need to think again about what other data you have which could be predictive of your target. If you can isolate some more data, you should add it to your data, and re-run model building.
  * **Get more examples:** it may be that your features should contain predictive information about your target, but this predictive signal is too complex for model building to uncover simply because it does not have enough examples. In this case, you should think about whether you can find more examples to use for training the model. If you can find more examples, you should add them to your data and re-run model building.

> **Note**  
> It is **not advisable** to re-run the model training without changing one of these things (read more about it [here](../theory/model-health.md#why-you-shouldnt-just-re-run-without-changing-anything)). Equally, you should not ignore bad model health unless some specific circumstances are met (read more about these [here](../theory/model-health.md#when-is-it-safe-to-ignore-bad-model-health).

## Model Influences

Model influences tell us **how important** each feature (column) in a data set is for predicting our target column. This can be immensely useful to decide what data should be collected and what can be left out.

Clicking on the Model Influences card takes us to a more detailed view. A few things to note here:

* In a classification model, we have the ability to switch between the target classes. Use the **Predicted Category** dropdown at the top of the page. In a regression problem, this is not present as we are predicting a continuous variable.
* The **Influence** bar tell us how important that feature is for predicting our target. Features are sorted by importance, so the most important will come out on top.
* The **plot** tells us how confident the model is that a **particular value** of that feature has a high influence on our prediction. See the worked example below. The plot comes in two forms, a **line plot** for continuous features, and a **box plot** for discrete features.

![Influences](../assets/trained-models/insight-influences.png)

### Worked Example

In the screen capture above, we are predicting whether a set of clients are going to churn from the mobile operator or remain with them. A prediction of churn is marked as "TRUE" and not churn as "FALSE".

According to the model generated for this example, the *customer service calls* variable is the most important feature, with an influence of 15% (of the total influence given by all features). Among the values of *customer service calls* present in the training data set, i.e. 0, 1, ..., 6 and 10, we can see that low values (closer to 0) swing the model more towards a prediction of false, i.e. not churning, than high values. We see this represented as a box plot because this is a categorical variable (i.e. it only has a set of discrete values). Note the variance, represented in the box plot by the top and bottom margins of the box (the first and third quartile) and in the continuous plot as the shaded surface around the median (the solid line), the edges of which also represent first and third quartiles. If this is large at a given value, it means that that value has less impact on the prediction.

The *international plan* feature in this example only has two values, and is therefore also *categorical*. We can see that having an international plan makes the customer more likely to churn, and vice versa.

Finally, the *total day charge*, third most important feature, has an influence of 9.9%, and similarly *total day minutes* of 9.8%. Again, on the range of values of *total day charge* present in the training data set, i.e. between 0 and 60, we can see that high values (towards 60) swing the model more towards a prediction of true, i.e. of churning, than low values.

## Model Influences on Tests

Another way to see the model influences is to inspect the output of a [Test](test.md), including a **Hold Out** test, as depicted below.

![Insights on Hold-out test](../assets/trained-models/insight-on-holdout.png)
