<a id="encoders-ref"></a>

# Encoding a column

See [encoding](../column_level_featurizinghtml.md#user-guide-encoders-index) for further details.

| [`StringEncoder`](generated/skrub.StringEncoderhtml.md#skrub.StringEncoder)             | Encode string features by using tf-idf vectorization and truncated singular     value decomposition (SVD).   |
|-----------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------|
| [`TextEncoder`](generated/skrub.TextEncoderhtml.md#skrub.TextEncoder)                   | Encode string features by applying a pretrained language model         downloaded from the HuggingFace Hub.  |
| [`MinHashEncoder`](generated/skrub.MinHashEncoderhtml.md#skrub.MinHashEncoder)          | Encode string categorical features by applying the MinHash method to n-gram     decompositions of strings.   |
| [`GapEncoder`](generated/skrub.GapEncoderhtml.md#skrub.GapEncoder)                      | Encode string columns by constructing latent topics.                                                         |
| [`SimilarityEncoder`](generated/skrub.SimilarityEncoderhtml.md#skrub.SimilarityEncoder) | Encode string categories to a similarity matrix, to capture fuzziness across a few categories.               |
| [`ToCategorical`](generated/skrub.ToCategoricalhtml.md#skrub.ToCategorical)             | Convert a string column to Categorical dtype.                                                                |
| [`DatetimeEncoder`](generated/skrub.DatetimeEncoderhtml.md#skrub.DatetimeEncoder)       | Extract temporal features such as month, day of the week, … from a datetime column.                          |
| [`SessionEncoder`](generated/skrub.SessionEncoderhtml.md#skrub.SessionEncoder)          | Add a session ID column to a dataframe based on time gaps and other columns.                                 |
| [`ToDatetime`](generated/skrub.ToDatetimehtml.md#skrub.ToDatetime)                      | Parse datetimes represented as strings and return `Datetime` columns.                                        |
| [`ToFloat`](generated/skrub.ToFloathtml.md#skrub.ToFloat)                               | Convert a column to 32-bit floating-point numbers.                                                           |
