<a id="installation-instructions"></a>

# Install

<div class="container mt-4">

<ul class="nav nav-pills nav-fill" id="installation" role="tablist">
    <li class="nav-item" role="presentation">
        <a class="nav-link active" id="pip-tab" data-bs-toggle="tab" data-bs-target="#pip-tab-pane" type="button" role="tab" aria-controls="pip" aria-selected="true">Using pip</a>
    </li>
    <li class="nav-item" role="presentation">
        <a class="nav-link" id="conda-tab" data-bs-toggle="tab" data-bs-target="#conda-tab-pane" type="button" role="tab" aria-controls="conda" aria-selected="false">Using conda</a>
    </li>
    <li class="nav-item" role="presentation">
        <a class="nav-link" id="mamba-tab" data-bs-toggle="tab" data-bs-target="#mamba-tab-pane" type="button" role="tab" aria-controls="mamba" aria-selected="false">Using mamba</a>
    </li>
    <li class="nav-item" role="presentation">
        <a class="nav-link" id="source-tab" data-bs-toggle="tab" data-bs-target="#source-tab-pane" type="button" role="tab" aria-controls="source" aria-selected="false">From source</a>
    </li>
</ul>

<div class="tab-content">
    <div class="tab-pane fade show active" id="pip-tab-pane" role="tabpanel" aria-labelledby="pip-tab" tabindex="0">
        <hr />
```console
pip install skrub -U
```

<br/>

**Deep learning dependencies**

Deep-learning based encoders like [`LLMEncoder`](reference/generated/skrub.LLMEncoderhtml.md#skrub.LLMEncoder) require installing optional
dependencies to use them. The following will install
[torch](https://pypi.org/project/torch/),
[transformers](https://pypi.org/project/transformers/),
and [sentence-transformers](https://pypi.org/project/sentence-transformers/).

```console
$ pip install skrub[transformers] -U
```

</div>
<div class="tab-pane fade" id="conda-tab-pane" role="tabpanel" aria-labelledby="conda-tab" tabindex="0">
    <hr />
```console
conda install -c conda-forge skrub
```

<br/>

**Deep learning dependencies**

Deep-learning based encoders like [`LLMEncoder`](reference/generated/skrub.LLMEncoderhtml.md#skrub.LLMEncoder) require installing optional
dependencies to use them. The following will install
[torch](https://anaconda.org/pytorch/pytorch),
[transformers](https://anaconda.org/conda-forge/transformers),
and [sentence-transformers](https://anaconda.org/conda-forge/sentence-transformers).

```console
$ conda install -c conda-forge skrub[transformers]
```

</div>
<div class="tab-pane fade" id="mamba-tab-pane" role="tabpanel" aria-labelledby="mamba-tab" tabindex="0">
    <hr />
```console
mamba install -c conda-forge skrub
```

<br/>

**Deep learning dependencies**

Deep-learning based encoders like [`LLMEncoder`](reference/generated/skrub.LLMEncoderhtml.md#skrub.LLMEncoder) require installing optional
dependencies to use them. The following will install
[torch](https://anaconda.org/pytorch/pytorch),
[transformers](https://anaconda.org/conda-forge/transformers),
and [sentence-transformers](https://anaconda.org/conda-forge/sentence-transformers).

```console
$ mamba install -c conda-forge skrub[transformers]
```

</div>
<div class="tab-pane fade" id="source-tab-pane" role="tabpanel" aria-labelledby="source-tab" tabindex="0">
    <hr />
<!-- Now that you're set up, -->
<!-- you may return to :ref:`writing your first pull request<writing-your-first-pull-request>` -->
<!-- and start coding! -->

## Installing from source

Clone your project from the main repository:

```console
git clone https://github.com/skrub-data/skrub.git
cd skrub
```

Install skrub in the current environment with development dependencies:

```console
pip install -e ".[dev]"
```

## Contributing to the library

To contribute to the library, check the [contributing guide](CONTRIBUTINGhtml.md#fork-project)

**Deep learning dependencies**

Deep-learning based encoders like [`LLMEncoder`](reference/generated/skrub.LLMEncoderhtml.md#skrub.LLMEncoder) require installing optional
dependencies to use them. The following will install
[torch](https://pypi.org/project/torch/),
[transformers](https://pypi.org/project/transformers/),
and [sentence-transformers](https://pypi.org/project/sentence-transformers/).

```console
$ pip install -e ".[transformers]"
```

    </div>
</div>
</div>
