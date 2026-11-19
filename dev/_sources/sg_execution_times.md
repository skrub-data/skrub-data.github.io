<a id="sphx-glr-sg-execution-times"></a>

# Computation times

**19:26.340** total execution time for 22 files **from all galleries**:

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

| Example                                                                                                                                                                                                                                                                  | Time      |   Mem (MB) |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------|------------|
| [Various string encoders: a sentiment analysis example](auto_examples/01_encoding/0020_text_with_string_encodershtml.md#sphx-glr-auto-examples-01-encoding-0020-text-with-string-encoders-py) (`../skrub/_docs/examples/01_encoding/0020_text_with_string_encoders.py`)  | 03:06.101 |      863.4 |
| [AggJoiner on a credit fraud dataset](auto_examples/03_joining/0070_join_aggregationhtml.md#sphx-glr-auto-examples-03-joining-0070-join-aggregation-py) (`../skrub/_docs/examples/03_joining/0070_join_aggregation.py`)                                                  | 02:55.141 |      959.2 |
| [Quick overview of DataOps](auto_tutorials/1111_data_ops_quick_tourhtml.md#sphx-glr-auto-tutorials-1111-data-ops-quick-tour-py) (`tutorials/1111_data_ops_quick_tour.py`)                                                                                                | 02:43.323 |      561.9 |
| [Multiple tables: building machine learning pipelines with DataOps](auto_examples/02_data_ops/1120_multiple_tableshtml.md#sphx-glr-auto-examples-02-data-ops-1120-multiple-tables-py) (`../skrub/_docs/examples/02_data_ops/1120_multiple_tables.py`)                    | 02:36.757 |      700.8 |
| [Tuning DataOps with Optuna](auto_examples/02_data_ops/1131_optuna_choiceshtml.md#sphx-glr-auto-examples-02-data-ops-1131-optuna-choices-py) (`../skrub/_docs/examples/02_data_ops/1131_optuna_choices.py`)                                                              | 01:28.410 |      575.1 |
| [Turn a DataOp into a “standard” scikit-learn estimator](auto_examples/02_data_ops/1170_sklearn_interface_learnerhtml.md#sphx-glr-auto-examples-02-data-ops-1170-sklearn-interface-learner-py) (`../skrub/_docs/examples/02_data_ops/1170_sklearn_interface_learner.py`) | 01:02.533 |      555.6 |
| [Encoding: from a dataframe to a numerical matrix for machine learning](auto_examples/01_encoding/0010_encodingshtml.md#sphx-glr-auto-examples-01-encoding-0010-encodings-py) (`../skrub/_docs/examples/01_encoding/0010_encodings.py`)                                  | 01:01.529 |      552.7 |
| [Fuzzy joining dirty tables with the Joiner](auto_examples/03_joining/0040_fuzzy_joininghtml.md#sphx-glr-auto-examples-03-joining-0040-fuzzy-joining-py) (`../skrub/_docs/examples/03_joining/0040_fuzzy_joining.py`)                                                    | 00:33.813 |      568   |
| [Hyperparameter tuning with DataOps](auto_examples/02_data_ops/1130_choiceshtml.md#sphx-glr-auto-examples-02-data-ops-1130-choices-py) (`../skrub/_docs/examples/02_data_ops/1130_choices.py`)                                                                           | 00:33.334 |      554.2 |
| [Skrub as preprocessing for Table Foundation Models](auto_examples/0120_tabiclhtml.md#sphx-glr-auto-examples-0120-tabicl-py) (`../skrub/_docs/examples/0120_tabicl.py`)                                                                                                  | 00:29.812 |     1371   |
| [SquashingScaler: Robust numerical preprocessing for neural networks](auto_examples/0100_squashing_scalerhtml.md#sphx-glr-auto-examples-0100-squashing-scaler-py) (`../skrub/_docs/examples/0100_squashing_scaler.py`)                                                   | 00:27.227 |      556   |
| [Subsampling for faster development](auto_examples/02_data_ops/1140_subsamplinghtml.md#sphx-glr-auto-examples-02-data-ops-1140-subsampling-py) (`../skrub/_docs/examples/02_data_ops/1140_subsampling.py`)                                                               | 00:20.364 |      556.9 |
| [Using PyTorch (via skorch) in DataOps](auto_examples/02_data_ops/1160_pytorchhtml.md#sphx-glr-auto-examples-02-data-ops-1160-pytorch-py) (`../skrub/_docs/examples/02_data_ops/1160_pytorch.py`)                                                                        | 00:20.363 |      565.5 |
| [Interpolation join: infer missing rows when joining two tables](auto_examples/03_joining/0080_interpolation_joinhtml.md#sphx-glr-auto-examples-03-joining-0080-interpolation-join-py) (`../skrub/_docs/examples/03_joining/0080_interpolation_join.py`)                 | 00:19.038 |      554.1 |
| [Spatial join for flight data: Joining across multiple columns](auto_examples/03_joining/0060_multiple_key_joinhtml.md#sphx-glr-auto-examples-03-joining-0060-multiple-key-join-py) (`../skrub/_docs/examples/03_joining/0060_multiple_key_join.py`)                     | 00:18.557 |     1084.2 |
| [Getting Started with skrub](auto_tutorials/0000_getting_startedhtml.md#sphx-glr-auto-tutorials-0000-getting-started-py) (`tutorials/0000_getting_started.py`)                                                                                                           | 00:18.346 |      553.2 |
| [Handling datetime features with the DatetimeEncoder](auto_examples/01_encoding/0030_datetime_encoderhtml.md#sphx-glr-auto-examples-01-encoding-0030-datetime-encoder-py) (`../skrub/_docs/examples/01_encoding/0030_datetime_encoder.py`)                               | 00:12.964 |      557.1 |
| [Sessions in time-based data: Predicting user purchases with the SessionEncoder](auto_examples/0110_session_encoderhtml.md#sphx-glr-auto-examples-0110-session-encoder-py) (`../skrub/_docs/examples/0110_session_encoder.py`)                                           | 00:10.652 |      552.7 |
| [Use case: developing locally and deploying to production](auto_examples/02_data_ops/1150_use_casehtml.md#sphx-glr-auto-examples-02-data-ops-1150-use-case-py) (`../skrub/_docs/examples/02_data_ops/1150_use_case.py`)                                                  | 00:10.192 |      688.5 |
| [Hands-On with Column Selection and Transformers](auto_examples/0010_apply_to_colshtml.md#sphx-glr-auto-examples-0010-apply-to-cols-py) (`../skrub/_docs/examples/0010_apply_to_cols.py`)                                                                                | 00:09.588 |      555.1 |
| [Deduplicating misspelled categories](auto_examples/03_joining/0050_deduplicationhtml.md#sphx-glr-auto-examples-03-joining-0050-deduplication-py) (`../skrub/_docs/examples/03_joining/0050_deduplication.py`)                                                           | 00:05.344 |      553.6 |
| [Deduplicating misspelled categories](auto_examples/0050_deduplicationhtml.md#sphx-glr-auto-examples-0050-deduplication-py) (`../skrub/_docs/examples/0050_deduplication.py`)                                                                                            | 00:02.953 |      553.3 |
