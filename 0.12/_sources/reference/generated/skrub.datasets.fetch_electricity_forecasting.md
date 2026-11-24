# fetch_electricity_forecasting

### skrub.datasets.fetch_electricity_forecasting(data_home=None)

Fetches the electricity usage dataset (forecasting), available at         [https://github.com/skrub-data/skrub-data-files](https://github.com/skrub-data/skrub-data-files)

This dataset was generated from data obtained from the
ENTSOE Open Data portal under the open source license (CC-BY 4.0):
[https://transparencyplatform.zendesk.com/hc/article_attachments/40921869376401](https://transparencyplatform.zendesk.com/hc/article_attachments/40921869376401)

and the Open Meteo Historical Weather API:
[https://open-meteo.com/en/docs/historical-forecast-api](https://open-meteo.com/en/docs/historical-forecast-api)
in accordance with the licence described:
[https://open-meteo.com/en/licence](https://open-meteo.com/en/licence)

This is a time-series forecasting use case. This dataset gives the total
electricity load in MW in France, covering a time range from
March 23, 2021 to May 31, 2025. In addition, the dataset contains
weather data for several cities within France.

It can be downloaded/loaded using the
sklearn.datasets.fetch_electricity_forecasting function.
Size on disk: 26MB.

* **Parameters:**
  **data_home: str or path, default=None**
  : The directory where to download and unzip the files.
* **Returns:**
  **Path**
  : The path to the electricity usage CSV file. These include the electricity load
    and weather data for several cities in France.

### Examples

```pycon
>>> import pandas as pd
>>> from pathlib import Path
>>> from skrub.datasets import fetch_electricity_forecasting
>>> path = fetch_electricity_forecasting()
>>> bayonne = pd.read_csv(path / "weather_bayonne.csv")
>>> bayonne.shape
(38688, 7)
```

For more detailed instructions on how to use this dataset, please refer
to the example here: [EuroSciPy2025](https://github.com/skrub-data/EuroSciPy2025)

<!-- !! processed by numpydoc !! -->
