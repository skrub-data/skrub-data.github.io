<a id="sphx-glr-sg-execution-times"></a>

# Computation times

**17:24.382** total execution time for 20 files **from all galleries**:

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

| Example                                                                                                                                                                                                                                                             | Time      |   Mem (MB) |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------|------------|
| [Quick overview of DataOps](auto_tutorials/1111_data_ops_quick_tour.md#sphx-glr-auto-tutorials-1111-data-ops-quick-tour-py) (`tutorials/1111_data_ops_quick_tour.py`)                                                                                               | 03:16.235 |      566.1 |
| [Various string encoders: a sentiment analysis example](auto_examples/01_encoding/0020_text_with_string_encoders.md#sphx-glr-auto-examples-01-encoding-0020-text-with-string-encoders-py) (`../skrub/_docs/examples/01_encoding/0020_text_with_string_encoders.py`) | 02:40.453 |     1374   |
| [Multiples tables: building machine learning pipelines with DataOps](auto_examples/02_data_ops/1120_multiple_tables.md#sphx-glr-auto-examples-02-data-ops-1120-multiple-tables-py) (`../skrub/_docs/examples/02_data_ops/1120_multiple_tables.py`)                  | 02:34.479 |      756.1 |
| [AggJoiner on a credit fraud dataset](auto_examples/03_joining/0070_join_aggregation.md#sphx-glr-auto-examples-03-joining-0070-join-aggregation-py) (`../skrub/_docs/examples/03_joining/0070_join_aggregation.py`)                                                 | 02:30.787 |      916.7 |
| [Tuning DataOps with Optuna](auto_examples/02_data_ops/1131_optuna_choices.md#sphx-glr-auto-examples-02-data-ops-1131-optuna-choices-py) (`../skrub/_docs/examples/02_data_ops/1131_optuna_choices.py`)                                                             | 01:14.270 |      625.8 |
| [Spatial join for flight data: Joining across multiple columns](auto_examples/03_joining/0060_multiple_key_join.md#sphx-glr-auto-examples-03-joining-0060-multiple-key-join-py) (`../skrub/_docs/examples/03_joining/0060_multiple_key_join.py`)                    | 00:42.932 |     2981.2 |
| [Interpolation join: infer missing rows when joining two tables](auto_examples/03_joining/0080_interpolation_join.md#sphx-glr-auto-examples-03-joining-0080-interpolation-join-py) (`../skrub/_docs/examples/03_joining/0080_interpolation_join.py`)                | 00:40.745 |     2678.9 |
| [Encoding: from a dataframe to a numerical matrix for machine learning](auto_examples/01_encoding/0010_encodings.md#sphx-glr-auto-examples-01-encoding-0010-encodings-py) (`../skrub/_docs/examples/01_encoding/0010_encodings.py`)                                 | 00:40.534 |      571.3 |
| [Hyperparameter tuning with DataOps](auto_examples/02_data_ops/1130_choices.md#sphx-glr-auto-examples-02-data-ops-1130-choices-py) (`../skrub/_docs/examples/02_data_ops/1130_choices.py`)                                                                          | 00:37.059 |      566.1 |
| [Fuzzy joining dirty tables with the Joiner](auto_examples/03_joining/0040_fuzzy_joining.md#sphx-glr-auto-examples-03-joining-0040-fuzzy-joining-py) (`../skrub/_docs/examples/03_joining/0040_fuzzy_joining.py`)                                                   | 00:32.695 |      579.7 |
| [Using PyTorch (via skorch) in DataOps](auto_examples/02_data_ops/1160_pytorch.md#sphx-glr-auto-examples-02-data-ops-1160-pytorch-py) (`../skrub/_docs/examples/02_data_ops/1160_pytorch.py`)                                                                       | 00:21.543 |      580   |
| [SquashingScaler: Robust numerical preprocessing for neural networks](auto_examples/0100_squashing_scaler.md#sphx-glr-auto-examples-0100-squashing-scaler-py) (`../skrub/_docs/examples/0100_squashing_scaler.py`)                                                  | 00:20.410 |      569.4 |
| [Getting Started with skrub](auto_tutorials/0000_getting_started.md#sphx-glr-auto-tutorials-0000-getting-started-py) (`tutorials/0000_getting_started.py`)                                                                                                          | 00:17.528 |      566.1 |
| [Subsampling for faster development](auto_examples/02_data_ops/1140_subsampling.md#sphx-glr-auto-examples-02-data-ops-1140-subsampling-py) (`../skrub/_docs/examples/02_data_ops/1140_subsampling.py`)                                                              | 00:16.902 |      589.8 |
| [Handling datetime features with the DatetimeEncoder](auto_examples/01_encoding/0030_datetime_encoder.md#sphx-glr-auto-examples-01-encoding-0030-datetime-encoder-py) (`../skrub/_docs/examples/01_encoding/0030_datetime_encoder.py`)                              | 00:09.204 |      569.4 |
| [Use case: developing locally and deploying to production](auto_examples/02_data_ops/1150_use_case.md#sphx-glr-auto-examples-02-data-ops-1150-use-case-py) (`../skrub/_docs/examples/02_data_ops/1150_use_case.py`)                                                 | 00:08.942 |      691.8 |
| [Sessions in time-based data: Predicting user purchases with the SessionEncoder](auto_examples/0110_session_encoder.md#sphx-glr-auto-examples-0110-session-encoder-py) (`../skrub/_docs/examples/0110_session_encoder.py`)                                          | 00:06.949 |      566.5 |
| [Hands-On with Column Selection and Transformers](auto_examples/0010_apply_to_cols.md#sphx-glr-auto-examples-0010-apply-to-cols-py) (`../skrub/_docs/examples/0010_apply_to_cols.py`)                                                                               | 00:06.609 |      566.1 |
| [Deduplicating misspelled categories](auto_examples/03_joining/0050_deduplication.md#sphx-glr-auto-examples-03-joining-0050-deduplication-py) (`../skrub/_docs/examples/03_joining/0050_deduplication.py`)                                                          | 00:04.418 |      566.1 |
| [Deduplicating misspelled categories](auto_examples/0050_deduplication.md#sphx-glr-auto-examples-0050-deduplication-py) (`../skrub/_docs/examples/0050_deduplication.py`)                                                                                           | 00:01.687 |      566.7 |
