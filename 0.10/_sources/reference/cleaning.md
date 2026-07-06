<a id="cleaning-ref"></a>

# Cleaning a dataframe

| [`SquashingScaler`](generated/skrub.SquashingScalerhtml.md#skrub.SquashingScaler)       | Perform robust centering and scaling followed by soft clipping.                                                |
|-----------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------|
| [`Cleaner`](generated/skrub.Cleanerhtml.md#skrub.Cleaner)                               | Column-wise consistency checks and sanitization of dtypes, null values and dates.                              |
| [`DropUninformative`](generated/skrub.DropUninformativehtml.md#skrub.DropUninformative) | Drop column if it is found to be uninformative according to various criteria.                                  |
| [`DropSimilar`](generated/skrub.DropSimilarhtml.md#skrub.DropSimilar)                   | Drop columns found too redundant to the rest of the dataframe, according to association defined by Cramér's V. |
| [`DurationToFloat`](generated/skrub.DurationToFloathtml.md#skrub.DurationToFloat)       | Convert duration columns to seconds.                                                                           |
| [`to_datetime`](generated/skrub.to_datetimehtml.md#skrub.to_datetime)                   | Convert DataFrame or column to Datetime dtype.                                                                 |
| [`deduplicate`](generated/skrub.deduplicatehtml.md#skrub.deduplicate)                   | Deduplicate categorical data by hierarchically clustering similar strings.                                     |
