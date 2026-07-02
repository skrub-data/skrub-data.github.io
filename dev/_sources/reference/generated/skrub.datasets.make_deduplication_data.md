# make_deduplication_data

### skrub.datasets.make_deduplication_data(examples, entries_per_example, prob_mistake_per_letter=0.2, random_state=None)

Duplicates examples with spelling mistakes.

Characters are misspelled with probability `prob_mistake_per_letter`.

* **Parameters:**
  **examples**
  : Examples to duplicate.

  **entries_per_example**
  : Number of duplications per example.

  **prob_mistake_per_letter**
  : Probability of misspelling a character in duplications.
    By default, 1/5 of the characters will be misspelled.

  **random_state**
  : Determines random number generation for dataset noise. Pass an int
    for reproducible output across multiple function calls.
* **Returns:**
  [`list`](https://docs.python.org/3/library/stdtypes.html#list) of [`str`](https://docs.python.org/3/library/stdtypes.html#str)
  : List of duplicated examples with spelling mistakes.

### Examples

```pycon
>>> from skrub.datasets import make_deduplication_data
>>> make_deduplication_data(["string1", "string2"],
...                         entries_per_example=[4, 5],
...                         random_state=9)
['btrwng1', 'string1',
'string1', 'string1',
'saoing2', 'string2',
'string2', 'string2',
'string2']
```

<!-- !! processed by numpydoc !! -->

## Gallery examples

<div class="sphx-glr-thumbnails">
<!-- thumbnail-parent-div-open --><div class="sphx-glr-thumbcontainer" tooltip="Real-world datasets often come with misspellings, for instance in manually inputted categorical variables. Such misspellings break data analysis steps that require exact matching, such as a GROUP BY operation.">  <div class="sphx-glr-thumbnail-title">Deduplicating misspelled categories</div>
</div>
<!-- thumbnail-parent-div-close --></div>
