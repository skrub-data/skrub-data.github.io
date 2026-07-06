# fetch_country_happiness

### skrub.datasets.fetch_country_happiness(data_home=None)

Fetch the happiness index dataset (regression) available at         [https://github.com/skrub-data/skrub-data-files](https://github.com/skrub-data/skrub-data-files)

This is a regression use-case, where the goal is to predict the happiness
index. The dataset contains data from the [2022 World Happiness Report](https://worldhappiness.report/), and from [the World Bank open data
platform](https://data.worldbank.org/). Size on disk: 64KB.

* **Parameters:**
  **data_home**
  : The directory where to download and unzip the files.
* **Returns:**
  **bunch**
  : A dictionary-like object with the following keys:
    <br/>
    happiness_report
    : Data from the world happiness report.
    <br/>
    GDP_per_capita
    : Data from the World Bank.
    <br/>
    life_expectancy
    : Data from the World Bank.
    <br/>
    legal_rights_index
    : Data from the World Bank.
    <br/>
    metadata
    : A dictionary containing the name of the dataset, a description
      and the sources.
    <br/>
    happiness_report_path
    : The path to the happiness report CSV file.
    <br/>
    GDP_per_capita_path
    : The path to the GDP per capita CSV file.
    <br/>
    life_expectancy_path
    : The path to the life expectancy CSV file.
    <br/>
    legal_rights_index_path
    : The path to the legal rights index CSV file.

<!-- !! processed by numpydoc !! -->

## Gallery examples

<div class="sphx-glr-thumbnails">
<!-- thumbnail-parent-div-open --><div class="sphx-glr-thumbcontainer" tooltip="Here we show how to combine data from different sources, with a vocabulary not well normalized.">  <div class="sphx-glr-thumbnail-title">Fuzzy joining dirty tables with the Joiner</div>
</div>
<!-- thumbnail-parent-div-close --></div>
