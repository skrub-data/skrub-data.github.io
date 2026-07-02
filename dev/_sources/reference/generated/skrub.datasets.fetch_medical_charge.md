# fetch_medical_charge

### skrub.datasets.fetch_medical_charge(data_home=None)

Fetches the medical charge dataset (regression), available at         [https://github.com/skrub-data/skrub-data-files](https://github.com/skrub-data/skrub-data-files)

Description of the dataset:
: The dataset provides information on inpatient discharges for Medicare
  fee-for-service beneficiaries. It includes information
  on utilization, payment (total payment and Medicare payment), and
  hospital-specific charges for the more than 3,000 U.S. hospitals that
  receive Medicare Inpatient Prospective Payment System (IPPS) payments.
  Size on disk: 36MB.

* **Parameters:**
  **data_home**
  : The directory where to download and unzip the files.
* **Returns:**
  **bunch**
  : A dictionary-like object with the following keys:
    <br/>
    medical_charge
    : The dataframe.
    <br/>
    X
    : Features, i.e. the dataframe without the target labels.
    <br/>
    y
    : Target labels.
    <br/>
    metadata
    : A dictionary containing the name, description, source and target.
    <br/>
    path
    : The path to the medical charge CSV file.

<!-- !! processed by numpydoc !! -->
