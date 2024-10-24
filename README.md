<h1 align="center">
    Tin Hau v0.8<br>A Building Time-series Foundation Model
</h1>

This is a Time-series Foundation Model for building load/energy forecasting. The model can conduct zero-shot or few-shot forecasting on diverse building energy data. 

![building_tsfm_overview](https://github.com/user-attachments/assets/57448f67-01eb-41a9-a171-9406c2cedcf1)

## Download this repository
Either clone the repository using the following command or directly download the zip file [`TinHau-forecasting.zip`](https://drive.google.com/file/d/1V5RYGlZlhYbnki-XROU-zC3Y8yDCh2wh/view?usp=sharing).
```bash
git clone https://github.com/Dylan0211/TinHau-forecasting.git
```
The supported python version is 3.8. Use the following command to install required packages.
```bash
pip install -r requirements.txt
```

## Evaluate the model

### Step 1: prepare data file
- The target building energy data should be stored in a `{target_building}.csv` file with two columns:
    - **Target column** is the column that will be forecasted (e.g., energy consumption).
    - **Timestamp column** is the column that contains the timestamp of the time-series.
- The `{target_building}.csv` file should be put under `dataset/{dataset_name}/`.
    - An example is the `Fox_office_Joy.csv` file under [`dataset/Genome/`](https://github.com/Dylan0211/Building-Time-series-Foundation-Model/tree/main/dataset/Genome).

### Step 2: config data loader
- The config of the target building should be specified for the data loader. Specifically, a config dict for the target building should be added to the `get_data()` function under [`tsfm_public/models/tinytimemixer/utils/ttm_utils.py`](https://github.com/Dylan0211/Building-Time-series-Foundation-Model/blob/main/tsfm_public/models/tinytimemixer/utils/ttm_utils.py). An example is shown as follows. Here, "Genome" is the name of the config dict which contains variables to be specified:
    - **"timestamp_column"** is the name of the column that contains the timestamp of the time-series.
    - **"target_columns"** is the name of the column that contains the variable to be forecasted.
    - **"split_config"** specifies the proportion of data used for training and testing.

  ```python
  def get_data(
    dataset_name: str,
    file_name: str,
    data_root_path: str,
    context_length,
    forecast_length,
    fewshot_fraction=1.0,
  ):
    print(context_length, forecast_length)

    config_map = {
        "Genome": {
            "dataset_path": file_name,
            "timestamp_column": "time",
            "target_columns": ["power"],
            "split_config": {
                "train": 0.1,
                "test": 0.9,
            },
        }
  ```

### Step 3: evaluate the model on the target building
- Evaluation can be conducted by running the [`eval.py`](https://github.com/Dylan0211/Building-Time-series-Foundation-Model/blob/main/eval.py). There are some parameters to be adjusted.
    - **dataset_name** is the name of your dataset.
    - **file_path** is the path to the `{target_building}.csv` file.
    - **evaluation_mode** is the way for evaluation. This can be either 'zeroshot' or 'fewshot'.
  
