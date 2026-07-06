# tabular_pipeline

### skrub.tabular_pipeline(estimator, , n_jobs=None)

Get a simple machine-learning pipeline for tabular data.

Given either a scikit-learn estimator or one of the special-cased strings
`'regressor'`, `'regression'`, `'classifier'`, `'classification'`, this
function creates a scikit-learn pipeline that extracts numeric features, imputes
missing values and scales the data if necessary, then applies the estimator.

#### NOTE
The heuristics used by the `tabular_pipeline`
to define an appropriate preprocessing based on the `estimator` may change
in future releases.

#### Versionchanged
Changed in version 0.6.0: The high cardinality encoder has been changed from
[`MinHashEncoder`](skrub.MinHashEncoderhtml.md#skrub.MinHashEncoder) to [`StringEncoder`](skrub.StringEncoderhtml.md#skrub.StringEncoder).

#### Versionchanged
Changed in version 0.7.0: The [`SquashingScaler`](skrub.SquashingScalerhtml.md#skrub.SquashingScaler) with `max_absolute_value=5` is now used instead of
[`StandardScaler`](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.StandardScaler.html#sklearn.preprocessing.StandardScaler) for centering and scaling
numerical features when using linear models.

* **Parameters:**
  **estimator**
  : The estimator to use as the final step in the pipeline. Based on the type of
    estimator, the previous preprocessing steps and their respective parameters are
    chosen. The possible values are:
    - `'regressor'` or `'regression'`: a
      [`HistGradientBoostingRegressor`](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.HistGradientBoostingRegressor.html#sklearn.ensemble.HistGradientBoostingRegressor) is used as the final
      step;
    - `'classifier'` or `'classification'`: a
      [`HistGradientBoostingClassifier`](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.HistGradientBoostingClassifier.html#sklearn.ensemble.HistGradientBoostingClassifier) is used as the final
      step;
    - a scikit-learn estimator: the provided estimator is used as the final step.

  **n_jobs**
  : Number of jobs to run in parallel in the [`TableVectorizer`](skrub.TableVectorizerhtml.md#skrub.TableVectorizer) step. `None`
    means 1 unless in a joblib `parallel_backend` context. `-1` means using all
    processors.
* **Returns:**
  [`Pipeline`](https://scikit-learn.org/stable/modules/generated/sklearn.pipeline.Pipeline.html#sklearn.pipeline.Pipeline)
  : A scikit-learn [`Pipeline`](https://scikit-learn.org/stable/modules/generated/sklearn.pipeline.Pipeline.html#sklearn.pipeline.Pipeline) chaining some preprocessing and
    the provided `estimator`.

### Notes

`tabular_pipeline` returns a scikit-learn [`Pipeline`](https://scikit-learn.org/stable/modules/generated/sklearn.pipeline.Pipeline.html#sklearn.pipeline.Pipeline) with
several steps:

- A [`TableVectorizer`](skrub.TableVectorizerhtml.md#skrub.TableVectorizer) transforms the tabular data into numeric features. Its
  parameters are chosen depending on the provided `estimator`.
- An optional [`SimpleImputer`](https://scikit-learn.org/stable/modules/generated/sklearn.impute.SimpleImputer.html#sklearn.impute.SimpleImputer) imputes missing values by their
  mean and adds binary columns that indicate which values were missing. This step is
  only added if the `estimator` cannot handle missing values itself.
- An optional [`SquashingScaler`](skrub.SquashingScalerhtml.md#skrub.SquashingScaler) centers and rescales the
  data. This step is not added (because it is unnecessary) when the `estimator` is
  a tree ensemble such as random forest or gradient boosting.
- The last step is the provided `estimator`.

The parameter values for the [`TableVectorizer`](skrub.TableVectorizerhtml.md#skrub.TableVectorizer) might differ depending on the
version of scikit-learn:

- support for categorical features in
  [`HistGradientBoostingClassifier`](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.HistGradientBoostingClassifier.html#sklearn.ensemble.HistGradientBoostingClassifier) and
  [`HistGradientBoostingRegressor`](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.HistGradientBoostingRegressor.html#sklearn.ensemble.HistGradientBoostingRegressor) was added in scikit-learn
  1.4. Therefore, before this version, a
  [`OrdinalEncoder`](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OrdinalEncoder.html#sklearn.preprocessing.OrdinalEncoder) is used for low-cardinality
  features.
- support for missing values in [`RandomForestClassifier`](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html#sklearn.ensemble.RandomForestClassifier)
  and [`RandomForestRegressor`](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestRegressor.html#sklearn.ensemble.RandomForestRegressor) was added in scikit-learn
  1.4. Therefore, before this version, a [`SimpleImputer`](https://scikit-learn.org/stable/modules/generated/sklearn.impute.SimpleImputer.html#sklearn.impute.SimpleImputer) is
  used to impute missing values.

Read more in the [User Guide](../../modules/default_wrangling/tabular_pipelinehtml.md#user-guide-tabular-pipeline).

### Examples

```pycon
>>> from skrub import tabular_pipeline
```

We can easily get a default pipeline for regression or classification:

```pycon
>>> tabular_pipeline('regression')
Pipeline(steps=[('tablevectorizer',
                 TableVectorizer(high_cardinality=StringEncoder(),
                                 low_cardinality=ToCategorical())),
                ('histgradientboostingregressor',
                 HistGradientBoostingRegressor(categorical_features='from_dtype'))])
```

When requesting a `'regression'`, the last step of the pipeline is set to a
[`HistGradientBoostingRegressor`](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.HistGradientBoostingRegressor.html#sklearn.ensemble.HistGradientBoostingRegressor).

```pycon
>>> tabular_pipeline('classification')
Pipeline(steps=[('tablevectorizer',
                 TableVectorizer(high_cardinality=StringEncoder(),
                                 low_cardinality=ToCategorical())),
                ('histgradientboostingclassifier',
                 HistGradientBoostingClassifier(categorical_features='from_dtype'))])
```

When requesting a `'classification'`, the last step of the pipeline is set to a
[`HistGradientBoostingClassifier`](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.HistGradientBoostingClassifier.html#sklearn.ensemble.HistGradientBoostingClassifier).

This pipeline can be applied to rich tabular data:

```pycon
>>> import pandas as pd
>>> X = pd.DataFrame(
...     {
...         "last_visit": ["2020-01-02", "2021-04-01", "2024-12-05", "2023-08-10"],
...         "medication": [None, "metformin", "paracetamol", "gliclazide"],
...         "insulin_prescriptions": ["N/A", 13, 0, 17],
...         "fasting_glucose": [35, 140, 44, 137],
...     }
... )
>>> y = [0, 1, 0, 1]
>>> X
   last_visit   medication insulin_prescriptions  fasting_glucose
0  2020-01-02          ...                   N/A               35
1  2021-04-01    metformin                    13              140
2  2024-12-05  paracetamol                     0               44
3  2023-08-10   gliclazide                    17              137
```

```pycon
>>> model = tabular_pipeline('classifier').fit(X, y)
>>> model.predict(X)
array([0, 0, 0, 0])
```

Rather than using the default estimator, we can provide our own scikit-learn
estimator:

```pycon
>>> from sklearn.linear_model import LogisticRegression
>>> model = tabular_pipeline(LogisticRegression())
>>> model.fit(X, y)
Pipeline(steps=[('tablevectorizer',
                TableVectorizer(datetime=DatetimeEncoder(periodic_encoding='spline'))),
                ('simpleimputer', SimpleImputer(add_indicator=True)),
                ('squashingscaler', SquashingScaler(max_absolute_value=5)),
                ('logisticregression', LogisticRegression())])
```

By applying only the first pipeline step we can see the transformed data that is
sent to the supervised estimator (see the [`TableVectorizer`](skrub.TableVectorizerhtml.md#skrub.TableVectorizer) documentation for
details):

```pycon
>>> model.named_steps['tablevectorizer'].transform(X)
   last_visit_year  last_visit_month  ...  insulin_prescriptions  fasting_glucose
0           2020.0               1.0  ...                    NaN             35.0
1           2021.0               4.0  ...                   13.0            140.0
2           2024.0              12.0  ...                    0.0             44.0
3           2023.0               8.0  ...                   17.0            137.0
```

The parameters of the [`TableVectorizer`](skrub.TableVectorizerhtml.md#skrub.TableVectorizer) depend on the provided `estimator`.

```pycon
>>> tabular_pipeline(LogisticRegression())
Pipeline(steps=[('tablevectorizer',
                TableVectorizer(datetime=DatetimeEncoder(periodic_encoding='spline'))),
                ('simpleimputer', SimpleImputer(add_indicator=True)),
                ('squashingscaler', SquashingScaler(max_absolute_value=5)),
                ('logisticregression', LogisticRegression())])
```

For a [`LogisticRegression`](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html#sklearn.linear_model.LogisticRegression), we get:

- a default configuration of the [`TableVectorizer`](skrub.TableVectorizerhtml.md#skrub.TableVectorizer) which is intended to work
  well for a wide variety of downstream estimators. The configuration adds
  `spline` periodic features to datetime columns.
- A [`SimpleImputer`](https://scikit-learn.org/stable/modules/generated/sklearn.impute.SimpleImputer.html#sklearn.impute.SimpleImputer), as the
  [`LogisticRegression`](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html#sklearn.linear_model.LogisticRegression) cannot handle missing values.
- A [`SquashingScaler`](skrub.SquashingScalerhtml.md#skrub.SquashingScaler) for centering and scaling
  numerical features.

On the other hand, For the [`HistGradientBoostingClassifier`](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.HistGradientBoostingClassifier.html#sklearn.ensemble.HistGradientBoostingClassifier)
(generated with the string `"classifier"`):

```pycon
>>> tabular_pipeline('classifier')
Pipeline(steps=[('tablevectorizer',
                 TableVectorizer(high_cardinality=StringEncoder(),
                                 low_cardinality=ToCategorical())),
                ('histgradientboostingclassifier',
                 HistGradientBoostingClassifier(categorical_features='from_dtype'))])
```

- A [`StringEncoder`](skrub.StringEncoderhtml.md#skrub.StringEncoder) is used as the `high_cardinality` encoder. This encoder
  strikes a good balance between quality and performance in most situations.
- The `low_cardinality` does not one-hot encode features. The
  [`HistGradientBoostingClassifier`](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.HistGradientBoostingClassifier.html#sklearn.ensemble.HistGradientBoostingClassifier) has built-in support for
  categorical data which is more efficient than one-hot encoding. Therefore the
  selected encoder, [`ToCategorical`](skrub.ToCategoricalhtml.md#skrub.ToCategorical), simply makes sure that those features have
  a categorical dtype so that the
  [`HistGradientBoostingClassifier`](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.HistGradientBoostingClassifier.html#sklearn.ensemble.HistGradientBoostingClassifier) recognizes them as such.
- There is no spline encoding of datetimes.
- There is no missing-value imputation because the classifier has its own (better)
  mechanism for dealing with missing values, and no standard scaling because it is
  unnecessary for tree ensembles.

<!-- !! processed by numpydoc !! -->

## Gallery examples

<div class="sphx-glr-thumbnails">
<!-- thumbnail-parent-div-open --><div class="sphx-glr-thumbcontainer" tooltip="This guide showcases some of the features of skrub. Much of skrub revolves around simplifying many of the tasks that are involved in pre-processing raw data into a format that shallow or classic machine-learning models can understand, that is, numerical data.">  <div class="sphx-glr-thumbnail-title">Getting Started with skrub</div>
</div><div class="sphx-glr-thumbcontainer" tooltip=" .. |ApplyToCols| replace:: ApplyToCols .. |StringEncoder| replace:: StringEncoder .. |SelectCols| replace:: SelectCols .. |DropCols| replace:: DropCols .. |TableVectorizer| replace:: TableVectorizer .. |OrdinalEncoder| replace:: OrdinalEncoder .. |PCA| replace:: PCA .. |Pipeline| replace:: Pipeline .. |ColumnTransformer| replace:: ColumnTransformer">  <div class="sphx-glr-thumbnail-title">Hands-On with Column Selection and Transformers</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="This example shows how to use |SessionEncoder| in a scikit-learn pipeline to create session-level features (sessionization) for conversion prediction, that is predicting whether a user session will eventually lead to a purchase.">  <div class="sphx-glr-thumbnail-title">Sessions in time-based data: Predicting user purchases with the SessionEncoder</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="This example shows how to transform a rich dataframe with columns of various types into a numerical matrix on which machine-learning algorithms can be applied. We study the case of predicting wages using the employee salaries dataset.">  <div class="sphx-glr-thumbnail-title">Encoding: from a dataframe to a numerical matrix for machine learning</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="Use case: developing locally and deploying to production">  <div class="sphx-glr-thumbnail-title">Use case: developing locally and deploying to production</div>
</div>
<!-- thumbnail-parent-div-close --></div>
