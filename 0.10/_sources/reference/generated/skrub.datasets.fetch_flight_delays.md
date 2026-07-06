# fetch_flight_delays

### skrub.datasets.fetch_flight_delays(data_home=None)

Fetch the flight delays dataset (regression) available at         [https://github.com/skrub-data/skrub-data-files](https://github.com/skrub-data/skrub-data-files)

This is a regression use-case, where the goal is to predict flight delays.
Size on disk: 657MB.

* **Parameters:**
  **data_home**
  : The directory where to download and unzip the files.
* **Returns:**
  **bunch**
  : A dictionary-like object with the following keys:
    <br/>
    flights
    : Information about the flights, including departure and
      arrival airports, and delay.
    <br/>
    airports
    : Information about airports, such as city and coordinates.
      The airport’s `iata` can be matched to the flights’ `Origin` and
      `Dest`.
    <br/>
    weather
    : Weather data that could be used to help improve the delay
      predictions. Note the weather data is not measured at the airports
      directly but at weather stations, whose location and information is
      provided in `stations`.
    <br/>
    stations
    : Information about the weather stations. `weather` and
      `stations` can be joined on their `ID` columns. Weather stations
      can only be matched to the nearest airport based on the latitude and
      longitude.
    <br/>
    metadata
    : A dictionary containing the name of the dataset.
    <br/>
    flights_path
    : The path to the flights CSV file.
    <br/>
    airports_path
    : The path to the airports CSV file.
    <br/>
    weather_path
    : The path to the weather CSV file.
    <br/>
    stations_path
    : The path to the stations CSV file.

<!-- !! processed by numpydoc !! -->

## Gallery examples

<div class="sphx-glr-thumbnails">
<!-- thumbnail-parent-div-open --><div class="sphx-glr-thumbcontainer" tooltip="Joining tables may be difficult if one entry on one side does not have an exact match on the other side.">  <div class="sphx-glr-thumbnail-title">Spatial join for flight data: Joining across multiple columns</div>
</div><div class="sphx-glr-thumbcontainer" tooltip="We illustrate the InterpolationJoiner, which is a type of join where values from the second table are inferred with machine-learning, rather than looked up in the table. It is useful when exact matches are not available but we have rows that are close enough to make an educated guess -- in this sense it is a generalization of a fuzzy_join.">  <div class="sphx-glr-thumbnail-title">Interpolation join: infer missing rows when joining two tables</div>
</div>
<!-- thumbnail-parent-div-close --></div>
