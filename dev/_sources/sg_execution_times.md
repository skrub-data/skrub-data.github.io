<a id="sphx-glr-sg-execution-times"></a>

# Computation times

**18:29.851** total execution time for 19 files **from all galleries**:

<style scoped>
<link href="https://cdnjs.cloudflare.com/ajax/libs/twitter-bootstrap/5.3.0/css/bootstrap.min.css" rel="stylesheet" />
<link href="https://cdn.datatables.net/1.13.6/css/dataTables.bootstrap5.min.css" rel="stylesheet" />
</style>
<script src="https://code.jquery.com/jquery-3.7.0.js"></script>
<script src="https://cdn.datatables.net/1.13.6/js/jquery.dataTables.min.js"></script>
<script src="https://cdn.datatables.net/1.13.6/js/dataTables.bootstrap5.min.js"></script>
<script type="text/javascript" class="init">
$(document).ready( function () {
    $('table.sg-datatable').DataTable({order: [[1, 'desc']]});
} );
</script>

| Example                                                                                                                                                                                                                                                     | Time      |   Mem (MB) |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------|------------|
| [Quick overview of DataOps](auto_tutorials/1111_data_ops_quick_tourhtml.md#sphx-glr-auto-tutorials-1111-data-ops-quick-tour-py) (`tutorials/1111_data_ops_quick_tour.py`)                                                                                   | 03:22.466 |      579.8 |
| [Various string encoders: a sentiment analysis example](auto_examples/01_encoding/0020_text_with_string_encodershtml.md#sphx-glr-auto-examples-01-encoding-0020-text-with-string-encoders-py) (`../examples/01_encoding/0020_text_with_string_encoders.py`) | 03:13.881 |     1404.1 |
| [Multiples tables: building machine learning pipelines with DataOps](auto_examples/02_data_ops/1120_multiple_tableshtml.md#sphx-glr-auto-examples-02-data-ops-1120-multiple-tables-py) (`../examples/02_data_ops/1120_multiple_tables.py`)                  | 02:47.628 |      781.2 |
| [AggJoiner on a credit fraud dataset](auto_examples/03_joining/0070_join_aggregationhtml.md#sphx-glr-auto-examples-03-joining-0070-join-aggregation-py) (`../examples/03_joining/0070_join_aggregation.py`)                                                 | 02:09.343 |      614.5 |
| [Tuning DataOps with Optuna](auto_examples/02_data_ops/1131_optuna_choiceshtml.md#sphx-glr-auto-examples-02-data-ops-1131-optuna-choices-py) (`../examples/02_data_ops/1131_optuna_choices.py`)                                                             | 01:18.298 |      640.9 |
| [Spatial join for flight data: Joining across multiple columns](auto_examples/03_joining/0060_multiple_key_joinhtml.md#sphx-glr-auto-examples-03-joining-0060-multiple-key-join-py) (`../examples/03_joining/0060_multiple_key_join.py`)                    | 00:48.731 |     2985.1 |
| [Encoding: from a dataframe to a numerical matrix for machine learning](auto_examples/01_encoding/0010_encodingshtml.md#sphx-glr-auto-examples-01-encoding-0010-encodings-py) (`../examples/01_encoding/0010_encodings.py`)                                 | 00:45.290 |      580.3 |
| [Interpolation join: infer missing rows when joining two tables](auto_examples/03_joining/0080_interpolation_joinhtml.md#sphx-glr-auto-examples-03-joining-0080-interpolation-join-py) (`../examples/03_joining/0080_interpolation_join.py`)                | 00:44.807 |     2231.5 |
| [Hyperparameter tuning with DataOps](auto_examples/02_data_ops/1130_choiceshtml.md#sphx-glr-auto-examples-02-data-ops-1130-choices-py) (`../examples/02_data_ops/1130_choices.py`)                                                                          | 00:36.961 |      579.7 |
| [Fuzzy joining dirty tables with the Joiner](auto_examples/03_joining/0040_fuzzy_joininghtml.md#sphx-glr-auto-examples-03-joining-0040-fuzzy-joining-py) (`../examples/03_joining/0040_fuzzy_joining.py`)                                                   | 00:34.662 |      587.2 |
| [Using PyTorch (via skorch) in DataOps](auto_examples/02_data_ops/1160_pytorchhtml.md#sphx-glr-auto-examples-02-data-ops-1160-pytorch-py) (`../examples/02_data_ops/1160_pytorch.py`)                                                                       | 00:23.573 |      592.7 |
| [SquashingScaler: Robust numerical preprocessing for neural networks](auto_examples/0100_squashing_scalerhtml.md#sphx-glr-auto-examples-0100-squashing-scaler-py) (`../examples/0100_squashing_scaler.py`)                                                  | 00:23.100 |      585   |
| [Getting Started with skrub](auto_tutorials/0000_getting_startedhtml.md#sphx-glr-auto-tutorials-0000-getting-started-py) (`tutorials/0000_getting_started.py`)                                                                                              | 00:20.684 |      579.8 |
| [Subsampling for faster development](auto_examples/02_data_ops/1140_subsamplinghtml.md#sphx-glr-auto-examples-02-data-ops-1140-subsampling-py) (`../examples/02_data_ops/1140_subsampling.py`)                                                              | 00:19.816 |      609.7 |
| [Handling datetime features with the DatetimeEncoder](auto_examples/01_encoding/0030_datetime_encoderhtml.md#sphx-glr-auto-examples-01-encoding-0030-datetime-encoder-py) (`../examples/01_encoding/0030_datetime_encoder.py`)                              | 00:11.581 |      583.8 |
| [Use case: developing locally and deploying to production](auto_examples/02_data_ops/1150_use_casehtml.md#sphx-glr-auto-examples-02-data-ops-1150-use-case-py) (`../examples/02_data_ops/1150_use_case.py`)                                                 | 00:09.582 |      707.3 |
| [Sessions in time-based data: Predicting user purchases with the SessionEncoder](auto_examples/0110_session_encoderhtml.md#sphx-glr-auto-examples-0110-session-encoder-py) (`../examples/0110_session_encoder.py`)                                          | 00:07.618 |      580.5 |
| [Hands-On with Column Selection and Transformers](auto_examples/0010_apply_to_colshtml.md#sphx-glr-auto-examples-0010-apply-to-cols-py) (`../examples/0010_apply_to_cols.py`)                                                                               | 00:07.101 |      583.1 |
| [Deduplicating misspelled categories](auto_examples/03_joining/0050_deduplicationhtml.md#sphx-glr-auto-examples-03-joining-0050-deduplication-py) (`../examples/03_joining/0050_deduplication.py`)                                                          | 00:04.729 |      580   |
