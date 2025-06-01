# 1. Tutorials

## 1.1. Blog Post Tutorials

1. Official Quickstart Guide

* Introduction to Qlib on Read the Docs: walks through installation, data preparation, training your first model, and backtesting with code snippets. 
[qlib.readthedocs.io](https://qlib.readthedocs.io/en/latest/introduction/introduction.html)

2. Demystifying Qlib: Your Guide to Microsoft’s AI-Driven Quantitative Investment Platform

* A concise Medium article covering Qlib’s architecture, core modules, and a simple example of training/prediction. 
[Medium](https://grepix.medium.com/demystifying-qlib-your-guide-to-microsofts-ai-driven-quantitative-investment-platform-c530fd632995)

3. Qlib: A Free AI Trading Beast Thanks to Microsoft

* In-depth Medium post showing how to configure Qlib, run the demo strategies, and explore the built-in Quant Model Zoo. 
[Medium](https://medium.com/coding-nexus/qlib-a-free-ai-trading-beast-thanks-to-microsoft-bd1921a41f84)

4. Make Money With This AI-Oriented Quantitative Investment Platform

* Beginner-friendly walkthrough on Medium, with CLI examples and tips for your first experiments. 
[Medium](https://medium.com/%40april-4/make-money-with-this-ai-oriented-quantitative-investment-platform-3afe5a0f45ed)

5. Vadim’s Blog: Qlib Introduction

* A hands-on post demonstrating a PyTorch MLP pipeline via YAML configs, covering data ingestion through evaluation. 
[vadim.blog](https://www.vadim.blog/page/2)


## 1.2. Video Tutorials

1. Qlib: AI-Driven Open-Source Platform for Quantitative Finance

* YouTube overview (Quantlab channel): installs Qlib, loads data, and runs a sample strategy. 
[YouTube](https://www.youtube.com/watch?v=BntwI2a3xWE)

2. Qlib: Machine Learning for Stock Prediction and Forecasting

* YouTube tutorial showing setup, data loading, and training an LSTM model end-to-end. 
[YouTube](https://www.youtube.com/watch?v=z6a4mQTkMwg)

3. Explore Qlib (Quantitative Libraries)

* Alphien channel video demonstrating Qlib’s feature API and basic Jupyter-based workflows. 
[YouTube](https://www.youtube.com/watch?v=pJpWVaFaW70)



# 2. Roadmap for Learning

## 2.1. Phase 1: Quickstart & Installation

**Goal**: Get Qlib installed, run your first demo, and understand the core components.

### 2.1.1. Official Quickstart Guide (Read the Docs)

Reference: https://github.com/shevapato2008/qlib

(1) Install Qlib and its dependencies

```shell
$ conda activate py311_tf218
# Install with pip
$ pip install pyqlib
# Install from source
$ pip install numpy
$ pip install --upgrade cython
$ git clone https://github.com/microsoft/qlib.git && cd qlib
$ pip install .  # `pip install -e .[dev]` is recommended for development. check details in docs/developer/code_standard_and_dev_guide.rst
```
```python
>>> import qlib
>>> qlib.__version__
'0.9.6.99'
```

(2) Data Preparation

**Get with module**
```shell
# get 1d data
python -m qlib.run.get_data qlib_data --target_dir ~/.qlib/qlib_data/cn_data --region cn
# get 1min data
python -m qlib.run.get_data qlib_data --target_dir ~/.qlib/qlib_data/cn_data_1min --region cn --interval 1min
```

**Get from source**
```shell
# get 1d data
python scripts/get_data.py qlib_data --target_dir ~/.qlib/qlib_data/cn_data --region cn
# get 1min data
python scripts/get_data.py qlib_data --target_dir ~/.qlib/qlib_data/cn_data_1min --region cn --interval 1min
```

**Automatic update of daily frequency data (from yahoo finance)**
* Automatic update of data to the "qlib" directory each trading day(Linux)
  * use crontab: `crontab -e`
  * set up timed tasks:

```shell
* * * * 1-5 python <script path> update_data_to_bin --qlib_data_1d_dir <user data dir>
```

* Manual update of data
  * `trading_date`: start of trading day
  * `end_date`: end of trading day (not included)

```shell
python scripts/data_collector/yahoo/collector.py update_data_to_bin --qlib_data_1d_dir <user data dir> --trading_date <start date> --end_date <end date>
```

**Checking the health of the data**

```shell
# base command
python scripts/check_data_health.py check_data \
    --qlib_dir ~/.qlib/qlib_data/cn_data

# add some parameters
python scripts/check_data_health.py check_data \
    --qlib_dir ~/.qlib/qlib_data/cn_data \
    --missing_data_num 30055 \
    --large_step_threshold_volume 94485 \
    --large_step_threshold_price 20
```


(3) Auto Quant Research Workflow
* Quant Research Workflow
```shell
cd examples  # Avoid running program under the directory contains `qlib`
qrun benchmarks/LightGBM/workflow_config_lightgbm_Alpha158.yaml

# qrun under debug mode
python -m pdb qlib/workflow/cli.py examples/benchmarks/LightGBM/workflow_config_lightgbm_Alpha158.yaml
```

* Graphical Reports Analysis:

Run `examples/workflow_by_code.ipynb` with jupyter notebook

### 2.1.2. Video: “Qlib: AI-Driven Open-Source Platform for Quantitative Finance” (Quantlab YouTube)

* Follow along the install steps
* See the demo strategy in action

> **Exercise**: Clone the Qlib repo, install via `pip install pyqlib`, and execute the demo script without modifications.


## 2.2. Phase 2: Core Concepts & Architecture

**Goal**: Understand Qlib’s design: data manager, dataset, models, backtester.

1. Medium: “Demystifying Qlib: Your Guide to Microsoft’s AI-Driven Quantitative Investment Platform”

* Learn about `D` (data API), `Dataset`, and the `Quant Model Zoo`

2. Medium: “Qlib: A Free AI Trading Beast Thanks to Microsoft”

* Deep dive into YAML workflows and model registry

> **Exercise**: Inspect the default YAML in `examples/high_freq` and trace how data → model → backtest are connected.


## 2.3. Phase 3: Data & Feature Pipelines

**Goal**: Master Qlib’s data ingestion, feature engineering, and storage layers.

1. Video: “Explore Qlib (Quantitative Libraries)” (Alphien channel)

* See how D.features and formula API work in Jupyter

2. Medium: “Vadim’s Blog: Qlib Introduction”

* Hands-on example building a PyTorch MLP via YAML

> **Exercise**:
> * Write a small script using `D.calendar`, `D.instruments`, and `D.features` to fetch momentum and volatility factors.
> * Save them to a local Parquet store and load via `QlibDataset`.


## 2.4. Phase 4: Model Development & Customization

**Goal**: Plug in your own algorithms and compare against built-in models.

1. Video: “Qlib: Machine Learning for Stock Prediction and Forecasting” (LSTM end-to-end)

* Learn how to configure and run a deep learning model

2. Medium: “Make Money With This AI-Oriented Quantitative Investment Platform”

* Examples of swapping in new models via the CLI

> **Exercise**:
> * Subclass BaseModel to wrap a simple scikit-learn or TensorFlow algorithm.
> * Register it in a YAML file and run `qrun train` + `qrun predict`.


## 2.5. Phase 5: Backtesting & Strategy Evaluation

**Goal**: Use Qlib’s backtest engine to validate strategies across different market regimes.

1. Revisit the Official Quickstart backtest section for walk-forward splits

2. Explore the built-in Quant Model Zoo strategies under `qlib.contrib.strategy`

> **Exercise**:
> * Configure a 5-fold time-series split (e.g., 2015–2019 train, 2020 test) in YAML.
> * Run `qrun backtest` and analyze Sharpe ratio, maximum drawdown, and turnover plots.


## 2.6. Phase 6: Online Serving & Automation

**Goal**: Move from offline experiments to a simple live-simulation or paper-trading setup.

1. Review OnlineManager in qlib.contrib.online.manager (see online rollout examples in the docs)

2. Integrate a market data stream (e.g., minute bars) into the online pipeline

> **Exercise**:
> * Set up a cron job or simple loop that:
>   * Fetches the latest data slice
>   * Calls `online_manager.run()` to generate signals
>   * Prints or logs target positions for paper-trading
