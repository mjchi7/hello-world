# hello-world
First step towards github!

An engineer graduate interested in fintech, software development, machine learning!
<h1 id="core">core</h1>


<h2 id="core.Pipeline">Pipeline</h2>

```python
Pipeline(self, data=None, label_name=None, got_label_data=True, encoder_name='ordinal_encoder', scaler_name='standard_scaler')
```
Pipeline object facilitate the encoding, scaling of features.

Attributes
----------
n_columns       :   list
    The list of numeric feature columns.
s_columns       :   list
    The list of string feature columns.


<h3 id="core.Pipeline.columns_inclusivity_check">columns_inclusivity_check</h3>

```python
Pipeline.columns_inclusivity_check(instance_columns, data_columns)
```
Checks whether instance_columns match data_columns or not.

Note: This test fails when data_columns do not at least contains all the
    instance_columns. If it contains all the instance_columns and some
    other columns, this test will not fail.

Parameters
----------
instance_columns    :   list
    The list of columns that must present in data columns.
data_columns        :   list
    The data columns that

Returns
-------
Boolean
    True if instance_columns is a subset of data_columns.


<h3 id="core.Pipeline.get_categories">get_categories</h3>

```python
Pipeline.get_categories(self, column=None)
```
Function to get categories information from encoders.

Parameters
----------
column  :   list
    If not None, the function will only return the categories for the
    specified columns.

Returns
-------
categories  :   list
    The categories extracted from encoders in list.

<h3 id="core.Pipeline.transform_data">transform_data</h3>

```python
Pipeline.transform_data(self, data, label_name=None, got_label_data=False, return_categories=False, return_label_data=False)
```
API for user to transform data using pre-fitted encoders and scalers.

Parameters
----------
data        :   pandas.DataFrame
    The data to be transformed using pre-fitted encoders/scalers.
label_name  :   string
    The name of the column in the dataframe that contains the label data.
got_label_data  :   Boolean
    A flag to indicate whether label data is present in the dataframe or not.
    if set to True and label_name is None, it is assumed that last column
    is label data.
return_categories   :   Boolean
    Whether or not to return the encoded s_data categories. If False,
    "categories" returned is None.
return_label_data   :   Boolean
    Whether or not to return the label data. If False, "label_data"
    returned is None.

Returns
-------
n_data_transformed  :   np.ndarray
    The n_data (numeric features) scaled with pre-fitted scalers.
s_data_transormed   :   np.ndarray
    The s_data (string features) encoded with pre-fitted encoders.
label_data          :   np.ndarray
    The y_data (label data) extracted from "data".



<h3 id="core.Pipeline.get_vocab_dictionary">get_vocab_dictionary</h3>

```python
Pipeline.get_vocab_dictionary(self)
```
API for user to retrieve dictionary size for each s_data column.

Parameters
----------
-

Returns
-------
vocab_dictionary    :   dict
    Dict object. Key = s_data column; Value = size of categories for
    that column.
    For example: {'gen_make_model': 130, 'gen_condition': 3}


<h3 id="core.Pipeline.save_pipeline">save_pipeline</h3>

```python
Pipeline.save_pipeline(self, model_folder)
```
API to save the pipeline instance in pickle form.

Parameters
----------
model_folder    :   string
    The folder to save this instance to.

Returns
-------
-


<h3 id="core.Pipeline.load_pipeline">load_pipeline</h3>

```python
Pipeline.load_pipeline(self, model_folder)
```
API to load the pipeline instance in pickle form.

Parameters
----------
model_folder    :   string
    The folder to save this instance to.

Returns
-------
-


<h1 id="items">items</h1>


<h2 id="items.PipelineItems">PipelineItems</h2>

```python
PipelineItems(self)
```
This class contains collection of scalers and encoders

Note: category_encoders are being used because sklearn.preprocessing
    can't handle unseen categories.

To use:
    Simply instantiate a PipelineItem object, and call the relevant function
    to get the the encoders/scalers.

To add items:
    To add encoders and scalers, the following rules and steps must be adhered.
    1. Make sure encoders and scalers to be added is compatible with
        sklearn.preprocessing encoders/scalers. i.e. contains the following
        functions: fit(X), transform(X), inverse_transform(X)
    2. For encoders, a function "get_categories" must be implemented by
        defining a custom class and inheriting the encoders you are trying
        to add. The output of such "get_categories" function is expected to
        be a list of categories. For implementation details, refers to
        class OrdinalEncoder(ce.ordinal.OrdinalEncoder): below.
    3. When both rules are followed, simply add in the choice in __init__
        function's self.encoders or self.scalers.


<h3 id="items.PipelineItems.get_scaler">get_scaler</h3>

```python
PipelineItems.get_scaler(self, scaler_name)
```
External API: Called externally in order to retrieve the desired
scaler.

Parameters
----------
scaler_name     :   string
The name of the scaler to be retrieved from "scalers" dictionary.
scaler_name must be present in scalers' list of keys.

Returns
-------
scaler          :
The scaler chosen.

Raises
------
KeyError
Raised when scaler_name is not found within scalers' keys list.

<h3 id="items.PipelineItems.get_encoder">get_encoder</h3>

```python
PipelineItems.get_encoder(self, encoder_name)
```
External API: Called externally in order to retrieve the desired
encoder.

Parameters
----------
encoder_name    :   string
The name of the scaler to be retrieved from "encoders" dictionary.
encoder_name must be present in encoders' list of keys.

Returns
-------
encoder        :
The encoder chosen.

Raises
------
KeyError
Raised when encoder_name is not found within encoders' keys list.

<h3 id="items.PipelineItems.get_all_scalers">get_all_scalers</h3>

```python
PipelineItems.get_all_scalers(self)
```
External API: Get all the available scalers.

Returns
-------
scalers     :   dict
    Dict object containing all the scalers defined in __init__

<h3 id="items.PipelineItems.get_all_encoders">get_all_encoders</h3>

```python
PipelineItems.get_all_encoders(self)
```
External API: Get all the available scalers.

Returns
-------
encoders    :   dict
    Dict object containing all the encoders defined in __init__

<h2 id="items.DummyScaler">DummyScaler</h2>

```python
DummyScaler(self)
```
A custom scaler that does nothing.

Purpose: Since scaling is optional in some of the ML application,
    DummyScaler is defined to do nothing while being able to fit in
    the pipeline process nicely.

<h2 id="items.OneHotEncoder">OneHotEncoder</h2>

```python
OneHotEncoder(self, verbose=0, cols=None, drop_invariant=False, return_df=True, impute_missing=True, handle_unknown='impute', use_cat_names=False)
```
Custom one_hot encoder implementation to include the "get_categories"
function in order to be able to fit in the pipeline.


<h2 id="items.OrdinalEncoder">OrdinalEncoder</h2>

```python
OrdinalEncoder(self, verbose=0, mapping=None, cols=None, drop_invariant=False, return_df=True, impute_missing=True, handle_unknown='impute')
```
Custom ordinal encoder implementation to include the "get_categories"
function in order to be able to fit in the pipeline.


