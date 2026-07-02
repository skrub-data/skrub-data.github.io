# Examples

<div id='sg-tag-list' class='sphx-glr-tag-list'></div><div class="sphx-glr-thumbnails">
<!-- thumbnail-parent-div-open --><div class="sphx-glr-thumbcontainer" tooltip=" .. |ApplyToCols| replace:: ApplyToCols .. |StringEncoder| replace:: StringEncoder .. |SelectCols| replace:: SelectCols .. |DropCols| replace:: DropCols .. |TableVectorizer| replace:: TableVectorizer .. |OrdinalEncoder| replace:: OrdinalEncoder .. |PCA| replace:: PCA .. |Pipeline| replace:: Pipeline .. |ColumnTransformer| replace:: ColumnTransformer">  <div class="sphx-glr-thumbnail-title">Hands-On with Column Selection and Transformers</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="The following example illustrates the use of the SquashingScaler, a transformer that can rescale and squash numerical features to a range that works well with neural networks and perhaps also other related models. Its basic idea is to rescale the features based on quantile statistics (to be robust to outliers), and then perform a smooth squashing function to limit the outputs to a pre-defined range. This transform has been found to even work well when applied to one-hot encoded features.">  <div class="sphx-glr-thumbnail-title">SquashingScaler: Robust numerical preprocessing for neural networks</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="This example shows how to use |SessionEncoder| in a scikit-learn pipeline to create session-level features (sessionization) for conversion prediction, that is predicting whether a user session will eventually lead to a purchase.">  <div class="sphx-glr-thumbnail-title">Sessions in time-based data: Predicting user purchases with the SessionEncoder</div>
</div>
<!-- thumbnail-parent-div-close --></div>

# Encoding features

<div id='sg-tag-list' class='sphx-glr-tag-list'></div><div class="sphx-glr-thumbnails">
<!-- thumbnail-parent-div-open --><div class="sphx-glr-thumbcontainer" tooltip="This example shows how to transform a rich dataframe with columns of various types into a numerical matrix on which machine-learning algorithms can be applied. We study the case of predicting wages using the employee salaries dataset.">  <div class="sphx-glr-thumbnail-title">Encoding: from a dataframe to a numerical matrix for machine learning</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="In this example, we explore the performance of string and categorical encoders available in skrub.">  <div class="sphx-glr-thumbnail-title">Various string encoders: a sentiment analysis example</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="In this example, we illustrate how to better integrate datetime features in machine learning models with the |DatetimeEncoder|.">  <div class="sphx-glr-thumbnail-title">Handling datetime features with the DatetimeEncoder</div>
</div>
<!-- thumbnail-parent-div-close --></div>

# Skrub DataOps

<div id='sg-tag-list' class='sphx-glr-tag-list'></div><div class="sphx-glr-thumbnails">
<!-- thumbnail-parent-div-open --><div class="sphx-glr-thumbcontainer" tooltip="In this example, we show how to build a DataOps plan to handle pre-processing, validation and hyperparameter tuning of a dataset with multiple tables.">  <div class="sphx-glr-thumbnail-title">Multiples tables: building machine learning pipelines with DataOps</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="A machine-learning pipeline typically contains values or choices which may influence its prediction performance, such as hyperparameters (e.g., the regularization parameter alpha of a RidgeClassifier, the learning_rate of a HistGradientBoostingClassifier), which estimator to use (e.g., RidgeClassifier or HistGradientBoostingClassifier), or which steps to include (e.g., should we join a table to bring additional information or not).">  <div class="sphx-glr-thumbnail-title">Hyperparameter tuning with DataOps</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="This example shows how to use Optuna to tune the hyperparameters of a skrub DataOp. As seen in the previous example, skrub DataOps can contain &quot;choices&quot;, objects created with choose_from, choose_int, choose_float, etc. and we can use hyperparameter search techniques to pick the best outcome for each choice. Performing this search with Optuna allows us to benefit from its many features, such as state-of-the-art search strategies, monitoring and visualization, stopping and resuming searches, and parallel or distributed computation.">  <div class="sphx-glr-thumbnail-title">Tuning DataOps with Optuna</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="Here we show how to use DataOp.skb.subsample to speed up interactive construction of a skrub DataOps plan by computing previews on a subsampled version of the original data.">  <div class="sphx-glr-thumbnail-title">Subsampling for faster development</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="Use case: developing locally and deploying to production">  <div class="sphx-glr-thumbnail-title">Use case: developing locally and deploying to production</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="This example shows how to wrap a PyTorch model with skorch and plug it into a skrub DataOps plan.">  <div class="sphx-glr-thumbnail-title">Using PyTorch (via skorch) in DataOps</div>
</div>
<!-- thumbnail-parent-div-close --></div>

# Joining tables with imperfect data

<div id='sg-tag-list' class='sphx-glr-tag-list'></div><div class="sphx-glr-thumbnails">
<!-- thumbnail-parent-div-open --><div class="sphx-glr-thumbcontainer" tooltip="Here we show how to combine data from different sources, with a vocabulary not well normalized.">  <div class="sphx-glr-thumbnail-title">Fuzzy joining dirty tables with the Joiner</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="Real-world datasets often come with misspellings, for instance in manually inputted categorical variables. Such misspellings break data analysis steps that require exact matching, such as a GROUP BY operation.">  <div class="sphx-glr-thumbnail-title">Deduplicating misspelled categories</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="Joining tables may be difficult if one entry on one side does not have an exact match on the other side.">  <div class="sphx-glr-thumbnail-title">Spatial join for flight data: Joining across multiple columns</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="Many problems involve tables whose entities have a one-to-many relationship. To simplify aggregate-then-join operations for machine learning, we can include the |AggJoiner| in our pipeline.">  <div class="sphx-glr-thumbnail-title">AggJoiner on a credit fraud dataset</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="We illustrate the InterpolationJoiner, which is a type of join where values from the second table are inferred with machine-learning, rather than looked up in the table. It is useful when exact matches are not available but we have rows that are close enough to make an educated guess -- in this sense it is a generalization of a fuzzy_join.">  <div class="sphx-glr-thumbnail-title">Interpolation join: infer missing rows when joining two tables</div>
</div>
<!-- thumbnail-parent-div-close --></div>
