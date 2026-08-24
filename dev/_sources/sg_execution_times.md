<a id="sphx-glr-sg-execution-times"></a>

# Computation times

**18:35.368** total execution time for 19 files **from all galleries**:

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
| [Various string encoders: a sentiment analysis example](auto_examples/01_encoding/0020_text_with_string_encodershtml.md#sphx-glr-auto-examples-01-encoding-0020-text-with-string-encoders-py) (`../examples/01_encoding/0020_text_with_string_encoders.py`) | 03:10.509 |     1302.8 |
| [Quick overview of DataOps](auto_tutorials/1111_data_ops_quick_tourhtml.md#sphx-glr-auto-tutorials-1111-data-ops-quick-tour-py) (`tutorials/1111_data_ops_quick_tour.py`)                                                                                   | 02:56.332 |      529.8 |
| [AggJoiner on a credit fraud dataset](auto_examples/03_joining/0070_join_aggregationhtml.md#sphx-glr-auto-examples-03-joining-0070-join-aggregation-py) (`../examples/03_joining/0070_join_aggregation.py`)                                                 | 02:55.514 |      718   |
| [Multiples tables: building machine learning pipelines with DataOps](auto_examples/02_data_ops/1120_multiple_tableshtml.md#sphx-glr-auto-examples-02-data-ops-1120-multiple-tables-py) (`../examples/02_data_ops/1120_multiple_tables.py`)                  | 02:42.708 |      693.4 |
| [Tuning DataOps with Optuna](auto_examples/02_data_ops/1131_optuna_choiceshtml.md#sphx-glr-auto-examples-02-data-ops-1131-optuna-choices-py) (`../examples/02_data_ops/1131_optuna_choices.py`)                                                             | 01:15.363 |      684   |
| [Spatial join for flight data: Joining across multiple columns](auto_examples/03_joining/0060_multiple_key_joinhtml.md#sphx-glr-auto-examples-03-joining-0060-multiple-key-join-py) (`../examples/03_joining/0060_multiple_key_join.py`)                    | 00:48.055 |     2945.6 |
| [Encoding: from a dataframe to a numerical matrix for machine learning](auto_examples/01_encoding/0010_encodingshtml.md#sphx-glr-auto-examples-01-encoding-0010-encodings-py) (`../examples/01_encoding/0010_encodings.py`)                                 | 00:45.839 |      532.5 |
| [Interpolation join: infer missing rows when joining two tables](auto_examples/03_joining/0080_interpolation_joinhtml.md#sphx-glr-auto-examples-03-joining-0080-interpolation-join-py) (`../examples/03_joining/0080_interpolation_join.py`)                | 00:40.374 |     2637.4 |
| [Fuzzy joining dirty tables with the Joiner](auto_examples/03_joining/0040_fuzzy_joininghtml.md#sphx-glr-auto-examples-03-joining-0040-fuzzy-joining-py) (`../examples/03_joining/0040_fuzzy_joining.py`)                                                   | 00:35.803 |      536.5 |
| [Hyperparameter tuning with DataOps](auto_examples/02_data_ops/1130_choiceshtml.md#sphx-glr-auto-examples-02-data-ops-1130-choices-py) (`../examples/02_data_ops/1130_choices.py`)                                                                          | 00:35.712 |      528.1 |
| [SquashingScaler: Robust numerical preprocessing for neural networks](auto_examples/0100_squashing_scalerhtml.md#sphx-glr-auto-examples-0100-squashing-scaler-py) (`../examples/0100_squashing_scaler.py`)                                                  | 00:26.493 |      537.2 |
| [Using PyTorch (via skorch) in DataOps](auto_examples/02_data_ops/1160_pytorchhtml.md#sphx-glr-auto-examples-02-data-ops-1160-pytorch-py) (`../examples/02_data_ops/1160_pytorch.py`)                                                                       | 00:25.390 |      540.2 |
| [Getting Started with skrub](auto_tutorials/0000_getting_startedhtml.md#sphx-glr-auto-tutorials-0000-getting-started-py) (`tutorials/0000_getting_started.py`)                                                                                              | 00:19.326 |      527.2 |
| [Subsampling for faster development](auto_examples/02_data_ops/1140_subsamplinghtml.md#sphx-glr-auto-examples-02-data-ops-1140-subsampling-py) (`../examples/02_data_ops/1140_subsampling.py`)                                                              | 00:16.815 |      551.4 |
| [Handling datetime features with the DatetimeEncoder](auto_examples/01_encoding/0030_datetime_encoderhtml.md#sphx-glr-auto-examples-01-encoding-0030-datetime-encoder-py) (`../examples/01_encoding/0030_datetime_encoder.py`)                              | 00:10.187 |      531.2 |
| [Use case: developing locally and deploying to production](auto_examples/02_data_ops/1150_use_casehtml.md#sphx-glr-auto-examples-02-data-ops-1150-use-case-py) (`../examples/02_data_ops/1150_use_case.py`)                                                 | 00:09.423 |      603   |
| [Sessions in time-based data: Predicting user purchases with the SessionEncoder](auto_examples/0110_session_encoderhtml.md#sphx-glr-auto-examples-0110-session-encoder-py) (`../examples/0110_session_encoder.py`)                                          | 00:09.258 |      528.1 |
| [Hands-On with Column Selection and Transformers](auto_examples/0010_apply_to_colshtml.md#sphx-glr-auto-examples-0010-apply-to-cols-py) (`../examples/0010_apply_to_cols.py`)                                                                               | 00:07.177 |      527.1 |
| [Deduplicating misspelled categories](auto_examples/03_joining/0050_deduplicationhtml.md#sphx-glr-auto-examples-03-joining-0050-deduplication-py) (`../examples/03_joining/0050_deduplication.py`)                                                          | 00:05.092 |      527.4 |
