# Chronic Kidney Disease Analysis
 An application that can be used across biomedical data science projects. The dataset used for the proof of concept can help physicians better understand chronic kidney disease (CKD) using numerous measurements and biomarkers that have been collected. 


## Description

This project is a display of what can be done with machine learning in order to make some analysis over data such as the Chronic Kidney Disease dataset.


## Getting Started

### Dependencies

* python 3.10


### Installing

* Clone the project
* Create a virtual environment

    ```bash
    # Linux
    python -m venv .venv

    # Windows
    py -m venv .venv
    ```

* Activate the virtual environment
  
    ```bash
    # Linux
    .venv/bin/activate

    # Windows (batch/cmd)
    .venv/Scripts/activate.bat

    # Windows (powershell)
    .venv/Scripts/Activate.ps1
    ```

* Install dependencies

    ```bash
    pip install -r requirements.txt
    ```

* Download the chronic kidney disease dataset from [here](https://archive.ics.uci.edu/dataset/336/chronic+kidney+disease) and extract it so that the .arff files are directly in the data/ folder.

* Unit testing
This code has been partially programmed following the Test Driven Developement approach. Here is the command to launch the tests script.
    ```bash
    python -m unittest discover
    ```


### Executing program

* import dependencies

    ```python
    from src import ETL_tool as ETL
    from sklearn.model_selection import train_test_split
    from autogluon.tabular import TabularDataset, TabularPredictor
    from pandas import DataFrame
    ```

* Extract Data

    ```python
    df = ETL.load_data("data/chronic_kidney_disease_full.arff")
    ```

* Prepare Data

    ```python
    df_train, df_test = train_test_split(df, test_size=0.33, random_state=1)
    test_data = df_test.drop(['class'], axis=1)
    ```

* Train Models

    ```python
    predictor = TabularPredictor(label='class').fit(train_data=df_train, verbosity=2, presets='best_quality')
    ```

* Display Training statistics

    ```python
    predictor.fit_summary()
    predictor.leaderboard(df_train, silent=True)
    predictor.feature_importance(data=df_train)
    ```

* Model Prediction Test and evaluation

    ```python
    y_pred = predictor.predict(test_data)
    df_pred = DataFrame(y_pred, columns=['class'])
    predictor.evaluate(df_test)
    ```

* Use Pretrained Model

    ```python
    predictor = TabularPredictor.load("AutogluonModels/ag-20230702_213736/")
    y_pred = predictor.predict(test_data)
    ```


## Authors

David Urban


## License

This project is licensed under the MIT License - see the LICENSE.md file for details


## Acknowledgments

* [DomPizzie](https://gist.github.com/DomPizzie/7a5ff55ffa9081f2de27c315f5018afc)
* [GhostofGoes](https://gist.github.com/GhostofGoes/94580e76cd251972b15b4821c8a06f59)
* [nicolasdao](https://gist.github.com/nicolasdao/a7adda51f2f185e8d2700e1573d8a633#mit-license)